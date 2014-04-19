title: Developing Flask Extensions

_"The idea of Flask is build a good foundation for all applications. Everything else is up to you or extensions."_ - Armin Ronacher

The app is the heart of Flask. All of the functionality is wrapped up in that.

extending Flask = changing app

You can change the app object by

* Hooking into request lifecycle
* Adding more resources
    * Jinja filters, tests, global variables
* Middleware
* Monkeypatching

Flask-FeatureFlags
* An extension with the power to turn parts of code on or off

Extensions are modules

    #!python
    from flask import Flask
    from flask.ext.featureflags as feature

    app = Flask(__name__)
    feature.FeatureFlag(app)

    app.config['FEATURE_FLAGS']['pycon'] = True

    @app.route("/")
    def hello():
        if feature.is_active('pycon'):
            return "Hello Pycon"
        else:
            return "Hello World"

    if __name__ == "__main__":
        app.run()

For referencing the active flag in the template

    {% if "pycon" is active_feature %}
        "Hi Pycon!"
    {% else %}
        "Hello World"
    {% endif %}

A simplified version of the FeatureFlags code

    #!python
    from flask import current_app
 
    class FeatureFlags(object):
     
        def __init__(self, app=None):
           
            if app is not None:
                self.init_app(app)
     
        def init_app(self, app):
     
            app.config.setdefault('FEATURE_FLAGS’, {})
            
            if hasattr(app, "add_template_test"):
                 app.add_template_test(self.in_config, name='active_feature')
            else:
                 app.jinja_env.tests['active_feature'] = self.in_config
     
            app.extensions['FeatureFlags'] = self 
     
        def in_config(self, feature):
            try:
                 return current_app.config['FEATURE_FLAGS'][feature]
            except (AttributeError, KeyError):
                 return False
     
    def is_active(feature): 
        feature_flagger = current_app.extensions['FeatureFlags']
        return feature_flagger.in_config(feature)


When developing extensions, you have to be more aware to different versions of Flask. TravisCI is useful for testing different versions of Python and Flask that are supported by the extension.

Things to watch out for when developing Flask Extensions

* use init_app because app factories
* be sure to set config defaults
* calling Flask hooks
* 0.10+ is a trap (lots of new features that are not in previous versions)
* how to get to our extension registry

Useful tools

* [Flask-DebugToolbar](https://github.com/mgood/flask-debugtoolbar) - Port of Django Debug Toolbar
* Flask-SeaSurf - request processing, cookies
* Flask-Admin - SQLAlchemy, blueprints, static files
* Flask-Classy - adds class-based views

