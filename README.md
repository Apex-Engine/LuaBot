# LuaBot
Discord is a freeware, multi-platform, voice and text client designed for gamers. It has a documented API that allows developers to make Discord bots for use on their servers.

Luvit is an open-source, asynchronous I/O Lua runtime environment. It is a combination of LuaJIT and libuv, layered with various libraries to provide server-side functionality similar to that of Node.js, but with Lua instead of JavaScript. Luvit's companion package manager, lit, makes it easy to set up the Luvit runtime and its published libraries.


The luvit CLI tool can be used as a scripting platform just like node. This can be used to run lua scripts as standalone servers, clients, or other tools.

This simple web server written in Luvit responds with Hello World for every request.

local http = require('http')

http.createServer(function (req, res)
  local body = "Hello world\n"
  res:setHeader("Content-Type", "text/plain")
  res:setHeader("Content-Length", #body)
  res:finish(body)
end):listen(1337, '127.0.0.1')

print('Server running at http://127.0.0.1:1337/')
And run this script using luvit.

> luvit server.lua
Server running at http://127.0.0.1:1337/
This script is a standalone HTTP server, there is no need for Apache or Nginx to act as host.

Using Third-Party Libraries

Luvit also has a package system that makes it easy to publish and consume libraries.

For example, @creationix has made a set of libraries that use coroutines instead of callbacks for async I/O and published these to lit.

Using lit install creationix/weblit to use an express-like framework built on top of coroutines.

> mkdir myapp && cd myapp
> lit install creationix/weblit
> vim server.lua
> luvit server.lua
The server.lua file will contain:

local weblit = require('weblit')

weblit.app
  .bind({host = "127.0.0.1", port = 1337})

  -- Configure weblit server
  .use(weblit.logger)
  .use(weblit.autoHeaders)

  -- A custom route that sends back method and part of url.
  .route({ path = "/:name"}, function (req, res)
    res.body = req.method .. " - " .. req.params.name .. "\n"
    res.code = 200
    res.headers["Content-Type"] = "text/plain"
  end)

  -- Start the server
  .start()
