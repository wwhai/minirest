
# 曾经在某个网红公司重构过的一个WEB框架，设计是对的（比后来接盘的人设计的好点吧），可惜出了意外中断了，留个纪念。大概率是永远都不写erlang了吧。

## Write a RESTful API Module

```erlang
%% --------------------------------------------------------------------
%% @author: wwhai
%% --------------------------------------------------------------------
-module(minirest_example_api).
-compile([export_all, nowarn_export_all]).
-api_tag("example").
% API
-http_api(#{path => "/example",
            func => example_get,
            method => 'GET',
            description => "example api",
            parameters =>[
                #{
                    in => "path",
                    name => "page",
                    required => true,
                    schema => #{
                        type => "integer",
                        maximum => 10,
                        minimum => 0
                    }
                },
                #{
                    in => "query",
                    name => "size",
                    required => true,
                    schema => #{
                        type => "integer",
                        maximum => 10,
                        minimum => 0
                    }
                }
            ],
            % for next edition
            responses =>[]
            }
).
%% ====================================================================
%% API functions
%% ====================================================================
example_get(RequestContext) ->
    ct:print("RequestContext => :~p~n", [RequestContext]),
    #{k => v}.

```

## Start the REST server

```erlang
application:ensure_all_started(minirest),
application:ensure_all_started(minirest_example),
application:set_env(minirest_example, modules, [minirest_example_api]),
minirest:start_listener("/api/v4",
                       minirest_example,
                       [{port, 9990}],
                       #{middlewares => [minirest_cors_middleware]},
                       [minirest_example]).
```

## Stop the REST server

```
minirest:stop_http(demo_rest_server).
```

## License

Apache License Version 2.0
