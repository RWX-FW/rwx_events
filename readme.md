# RWE Library Documentation

RWE is a lightweight networking library for FiveM that provides a structured way to handle client-server communication through events and callbacks.

## Installation

1. Add the resource to your server
2. Import the library in your resource:
```lua
shared_script '@rwx_events/init.lua'
```

## Event System

### Triggering Events

```lua
-- Client -> Server
rwe.event.trigger('eventName', ...)

-- Server -> Client
rwe.event.trigger('eventName', playerId, ...)
```

### Registering Event Handlers

```lua
-- Basic event registration
rwe.event.register('eventName', function(...)
    -- Handle event
end)

-- With configuration
rwe.event.register('eventName', function(...)
    -- Handle event
end, {
    serverOnly = true, -- Only allow server-triggered events
    invoke = 'resourceName', -- Restrict to specific resource
    -- or you can use a table to restrict to multiple resources
    invoke = {
        ['resourceName1'] = true,
        ['resourceName2'] = true,
    }
})
```

## Callback System

### Making Callback Requests

```lua
-- Async with callback function
rwe.callback('eventName', nil, function(result, ...)
    -- Handle response
end, ...)

-- Using await
local result = rwe.callback.await('eventName', nil, ...)

-- With rate limiting
local result = rwe.callback.await('eventName', 1000, ...) -- 1 second delay
```

### Registering Callback Handlers

```lua
-- Client-side
rwe.callback.register('eventName', function(...)
    return ... -- Return response data
end)

-- Server-side
rwe.callback.register('eventName', function(source, ...)
    return ... -- Return response data
end)
```

## Configuration Options

### Event Configuration

```lua
{
    serverOnly = boolean, -- Only allow server-triggered events
    invoke = string|table -- Restrict which resources can trigger the event
}
```

### Global Settings

```lua
-- Event timeout (in milliseconds)
SetConvar('eventTimeout', '300000') -- Default: 5 minutes
```

## Best Practices

1. Always use rate limiting for frequent callbacks
2. Implement error handling for timeouts
3. Use `serverOnly` for sensitive operations
4. Validate data on both client and server
5. Use typed definitions for better code completion

## Server vs Client

The library automatically detects the context and provides appropriate methods. Server-side callbacks receive the source as the first parameter, while client-side ones don't.
