# Prerender service

# Setup

`npm install`


Add the following into your nginx config (note the service runs on `:4000`):

      # Support page prerendering for web crawlers
      if ($args ~ "(.*)?&?_escaped_fragment_=(.*)") {
          set $args $1;
          set $cleanurl $uri$is_args$1;
          set $cleanservername $http_host;
          rewrite ^ /snapshot${cleanurl};
      }
      location /snapshot {
          proxy_set_header X-Rewrite-CleanHOST $cleanservername;
          proxy_set_header X-Rewrite-CleanURI $cleanurl;
          proxy_set_header X-Rewrite-URI $request_uri;
          proxy_pass http://localhost:4000;
          proxy_connect_timeout 60s;
      }


To start the service run `node prerender`
