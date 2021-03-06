
### Lumen exceptions render function to catch, log and return appropriate error messages

#### NB: Log is a model for logging the errors in your Database
```php
public function render($request, Exception $e)
{
        $response = [
            'success' => false,
            'status' => 400,
            'message' => ""
        ];

       if ($e instanceof ValidationException) {
            $response['status'] = Response::HTTP_METHOD_NOT_ALLOWED;
            $response['message'] = "Http method not allowed.";
            Log::create([
                'status' => Response::HTTP_METHOD_NOT_ALLOWED,
                'message' => $e->getMessage(),
                'data' => $e->getTraceAsString(),
                'success' => false
            ]);
        } elseif ($e instanceof NotFoundHttpException) {
            $response['status'] = Response::HTTP_NOT_FOUND;
            $response['message'] = "Resource not Found";
            Log::create([
                'status' => Response::HTTP_NOT_FOUND,
                'message' => $e->getMessage(),
                'data' => $e->getTraceAsString(),
                'success' => false
            ]);
        } elseif ($e instanceof AuthorizationException) {
            $response['status'] = Response::HTTP_FORBIDDEN;
            Log::create([
                'status' => Response::HTTP_FORBIDDEN,
                'message' => $e->getMessage(),
                'data' => $e->getTraceAsString(),
                'success' => false
            ]);
        } elseif ($e instanceof ValidationException) {
            $response['status'] = Response::HTTP_BAD_REQUEST;
            $response['message'] = "Bad Request sent";
            Log::create([
                'status' => Response::HTTP_BAD_REQUEST,
                'message' => $e->getMessage(),
                'data' => $e->getTraceAsString(),
                'success' => false
            ]);
        } elseif ($e instanceof ValidationException) {
            $response['status']   = 422;
            $response['message'] = "Validation error";
            $response['errors'] = $e->validator->errors();
            Log::create([
                'status' => 422,
                'message' => $e->getMessage(),
                'data' => $e->validator->errors(),
                'success' => false
            ]);
        } elseif ($e instanceof UnsupportedMediaTypeHttpException) {
            $response['status'] = Response::HTTP_UNSUPPORTED_MEDIA_TYPE;
            Log::create([
                'status' => Response::HTTP_UNSUPPORTED_MEDIA_TYPE,
                'message' => "Unsupported media type error",
                'data' => $e->getTraceAsString(),
                'success' => false
            ]);
        }
        elseif ($e instanceof TokenExpiredException ) {
            $response['status'] = Response::HTTP_UNAUTHORIZED;
            $response['message'] = "Token expired";
            Log::create([
                'status' => Response::HTTP_UNAUTHORIZED,
                'message' => "Token expired",
                'data' => $e->getTraceAsString(),
                'success' => false
            ]);
        } else if ($e instanceof TokenInvalidException) {
            $response['status'] = Response::HTTP_UNAUTHORIZED;
            $response['message'] = "Token invalid";

            Log::create([
                'status' => Response::HTTP_UNAUTHORIZED,
                'message' => "Token invalid",
                'data' => $e->getTraceAsString(),
                'success' => false
            ]);
        }
        elseif ($e instanceof HttpResponseException) {
            $response['status']   = Response::HTTP_INTERNAL_SERVER_ERROR;
            $response['message'] = "Internal Server Error. Please contact the Server Administrator";
            Log::create([
                'status' => Response::HTTP_INTERNAL_SERVER_ERROR,
                'message' => $e->getMessage(),
                'data' => $e->getTraceAsString(),
                'success' => false
            ]);
        }

            return response()->json($response, $response['status']);
  }
```
