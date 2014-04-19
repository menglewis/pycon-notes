title: Writing RESTful Web Services with Flask

###Read Resource Collections

    #!python
    from flask import jsonify

    @api.route('/students/', methods=['GET'])
    def get_students():
        return jsonify({'urls': [s.get_url() for s in Student.query.all()]})


* Get requests are used to read collection of resources
* The response includes the URLS for all the resources in the collection
* The individual responses have URIs for the invidivual resources instead of all of the data in order to take advantage of caching in just on place (the individual resource)

###Read Individual Resource
    
    #!python
    @api.route('/students/<int:id>', methods=['GET'])
    def get_student(id):
        s = Student.query.get_or_404(id)
        return jsonify(s.to_json())

* GET requests are also used to read individual resource
* Routes can match dynamic components such as the id
* Errors are handled in lower level code with exceptions making high level code straightforward and clean
* The to_json() helper method in the model calss returns the JSON rep as a Python dict
* A successful reponse has status code 200 (OK)

###Per-Resource Collections
    
    #!python
    @api.route('/students/<int:id>/registrations/', methods=['GET'])
    def get_student_registrations(id):
        s = Student.query.get_or_404(id)
        return jsonify()

###Create a Resouce

    #!python
    @api.route('/students/', methods=['POST'])
    @json
    def new_student():
        student = Student().from_json(request.json)
        db.session.add(student)
        db.session.commit()
        return {}, 201, {'Location': student.get_url()}

###Update a Resource

    #!python
    @api.route('/students/<int:id>', methods=['PUT'])
    @json
    def edit_student(id):
        student = Student.query.get_or_404(id)
        student.from_json(request.json)
        db.session.add(student)
        db.session.commit()
        return {}

###Exporting JSON Representations

    #!python
    class Student(db.Model):
        # ...
        def get_url(self):
            return url_for('api.get_student', id=self.id, _external=True)

        def to_json(self):
            return {
                'url': self.get_url(),
                'name': self.name,
                'registrations': url_for('api.get_student_registrations',
                                         id=self.id, _external=True)
            }

###Importing JSON Representations

    #!python
    class Student(db.Model):
        # ...

        def from_json(self, json):
            try:
                self.name = json['name']
            except KeyError as e:
                raise ValidationError('Invalid student: missing ' + e.args[0])
            return self

* Models update themselves from their JSON representation

###Error Handling

    #!python
    def bad_request(message):
        response = jsonify({'status': 400, 'error': 'bad request',
                            'message': message})
        response.status_code = 400
        return response

    def not_found(message):
        response = jsonify({'status': 404, 'error': 'not found',
                            'message': message})
        response.status_code = 404
        return response

##Unit Testing

    #!python
    class TestAPI(unittest.TestCase):
        # ...
        def test_students(self):
            # get collection
            rv, json = self.client.get('/api/v1.0/students/')
            self.assertTrue(rv.status_code == 200)
            self.assertTrue(json['urls'] == [])

            # create new
            rv, json = self.client.post('/api/v1.0/students/',
                                        data={'name': 'susan'})
            self.assertTrue(rv.status_code == 201)
            susan_url = rv.headers['Location']

            # get
            rv, json = self.client.get(susan_url)
            self.assertTrue(rv.status_code == 200)
            self.assertTrue(json['name'] == 'susan')
            self.assertTrue(json['url'] == susan_url)
        

* The testing API client is built on top of Flask's own test client

##Advanced APIs with Flask

###Authentication
Flask-HTTPAuth - extention that takes care of HTTP headers and handles HTTP Basic Auth Protocol

* The auth.verify_password decorator register a password verification function, invoked before decorated routes occur

Protecting API routes
@auth.login_required

Bad or missing credentials return a 401 (unauthorized) response

    #!python
    @api.before_request
    @auth.login_required
    def before_request():
        pass

Global API Protection

@api.before_request

@auth.login_required

###Using Auth tokens

* The user model generates and validates auth tokens
* Many ways to generate tokens, one possibility is itsdangerous
* The verify_password decorated function validates token given in the username field. The password field is not used
* Tokens can be given to clients through tempalate rendering or through a password protected route

Example: 

    #!python
    @token_auth.verify_password
    def verify_password(username_or_token, password):
        g.user = User.query.filter_by(username=username_or_token).first()
        if not g.user:
            return False
        return g.user.verify_password(password)

    @auth.verify_password
    def verify_password(username_or_token, password):
        if current_app.config['USE_TOKEN_AUTH']:
            # token authentication
            g.user = User.verify_auth_token(username_or_token)
            return g.user is not None
        else:
            # username/password authentication
            g.user = User.query.filter_by(username=username_or_token).first()
            return g.user is not None and g.user.verify_password(password)


###Automatic JSON Responses
* jsonify and to_json() functions are used a lot
* A custom decorator can make these calls for us

Example:

    #!python
    def json(f):
        @functools.wraps(f)
        def wrapped(*args, **kwargs):
            rv = f(*args, **kwargs)
            status_or_headers = None
            headers = None
            if isinstance(rv, tuple):
                rv, status_or_headers, headers = rv + (None,) * (3 - len(rv))
            if isinstance(status_or_headers, (dict, list)):
                headers, status_or_headers = status_or_headers, None
            if not isinstance(rv, dict):
                rv = rv.to_json()
            rv = jsonify(rv)
            if status_or_headers is not None:
                rv.status_code = status_or_headers
            if headers is not None:
                rv.headers.extend(headers)
            return rv
        return wrapped

###Rate Limiting
    #!python
    @rate_limit(limit=5, per=15) # Limit of 5 requests per 15 seconds

* The rate limiting logic is wrapped in a custom decorator
* The limits are enformeced individuall for each client based on IP address
* The decorator can be applied individually to routes or globally to a before_request handler
* This implementation using Redis for storage
* Request that is above the limit is returned with 429 (Too many requests)

Example:
    
    #!python
    def rate_limit(limit, per, scope_func=lambda: request.remote_addr):
        def decorator(f):
            @functools.wraps(f)
            def wrapped(*args, **kwargs):
                if current_app.config['USE_RATE_LIMITS']:
                    key = 'rate-limit/%s/%s/' % (f.__name__, scope_func())
                    limiter = RateLimit(key, limit, per)
                    if not limiter.over_limit:
                        rv = f(*args, **kwargs)
                    else:
                        rv = too_many_requests('You have exceeded your request rate')
                    #rv = make_response(rv)
                    g.headers = {
                        'X-RateLimit-Remaining': str(limiter.remaining),
                        'X-RateLimit-Limit': str(limiter.limit),
                        'X-RateLimit-Reset': str(limiter.reset)
                    }
                    return rv
                else:
                    return f(*args, **kwargs)
            return wrapped
        return decorator


###Pagination
* Large collections are paginated to avoid large queries
* The pagination boilerplate is hidden in a custom decorator
* The route function retursn the database query object and the decorator handles the rest

Example:

    #!python
    @api.route('/students/<int:id>', methods=['GET'])
    @json
    def get_student(id):
        return Student.query.get_or_404(id)

    def paginate(max_per_page=10):
        def decorator(f):
            @functools.wraps(f)
            def wrapped(*args, **kwargs):
                page = request.args.get('page', 1, type=int)
                per_page = min(request.args.get('per_page', max_per_page,
                                                type=int), max_per_page)
                query = f(*args, **kwargs)
                p = query.paginate(page, per_page)
                pages = {'page': page, 'per_page': per_page,
                         'total': p.total, 'pages': p.pages}
                if p.has_prev:
                    pages['prev'] = url_for(request.endpoint, page=p.prev_num,
                                            per_page=per_page,
                                            _external=True, **kwargs)
                else:
                    pages['prev'] = None
                if p.has_next:
                    pages['next'] = url_for(request.endpoint, page=p.next_num,
                                            per_page=per_page,
                                            _external=True, **kwargs)
                else:
                    pages['next'] = None
                pages['first'] = url_for(request.endpoint, page=1,
                                         per_page=per_page, _external=True,
                                         **kwargs)
                pages['last'] = url_for(request.endpoint, page=p.pages,
                                        per_page=per_page, _external=True,
                                        **kwargs)
                return jsonify({
                    'urls': [item.get_url() for item in p.items],
                    'meta': pages
                })
            return wrapped
        return decorator


###HTTP Caching
* Some responses should never be cached (like the request token view)
* The no_cache decorator adds the appropriate Cache-Control headers

* The cache_control header takes HTTP caching directives as arguments and adds them to the response

Example:

    #!python
    def etag(f):
        @functools.wraps(f)
        def wrapped(*args, **kwargs):
            # only for HEAD and GET requests
            assert request.method in ['HEAD', 'GET'],\
                '@etag is only supported for GET requests'
            rv = f(*args, **kwargs)
            rv = make_response(rv)
            etag = '"' + hashlib.md5(rv.get_data()).hexdigest() + '"'
            rv.headers['ETag'] = etag
            if_match = request.headers.get('If-Match')
            if_none_match = request.headers.get('If-None-Match')
            if if_match:
                etag_list = [tag.strip() for tag in if_match.split(',')]
                if etag not in etag_list and '*' not in etag_list:
                    rv = precondition_failed()
            elif if_none_match:
                etag_list = [tag.strip() for tag in if_none_match.split(',')]
                if etag in etag_list or '*' in etag_list:
                    rv = not_modified()
            return rv
        return wrapped

###Conditional Requests
Etag Headers

* HTTP caches use entity tags to identify resources
* If the etag changes, the client know the version of the reousce it has is stale
* the etag custom decorator generates etags for responses to GET requests and handles the conditional request that use them
* the etag decorator adds the MD5 of the response as the Etag header
