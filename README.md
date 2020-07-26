# slidebar

#### *an animated widget bar for Awesome WM*

slidebar is an easy to use, configurable, multi-purpose, animated widget bar written in Lua (making use of the Awesome WM API). Really it is just a  bar that stays out of view until you move the mouse cursor within its activation region, at which point it slides out. Once you move the mouse cursor away it slides out of view. Here you can see it in action:

![Demo](https://raw.githubusercontent.com/mut-ex/awesome-wm-slidebar/master/demo.gif)


## Installation

The easiest was to get slidebar is to clone this repository into your awesome configuration folder like so:

```shell
$ cd ~/.config/awesome
$ git clone https://github.com/mut-ex/awesome-wm-slidebar.git slidebar
```



## Usage

To use slidebar, you first need to load the module. You can do so by putting the following line in your **rc.lua** (or whichever file you intend to create the bar in):

```lua
local slidebar = require('slidebar')
```



Then you can create a new slide bar instance like so:

```lua
local myslidebar = slidebar {
    -- bg = "#282a36",
    -- position = "top",
    -- size = 40,
    -- size_activator = 1
    -- show_delay = 0.25,
    -- hide_delay = 0.5,
    -- easing = 2,
    -- delta = 1,

    -- screen = nil
}
```

The commented out lines show the **default** values for each of the configuration parameters:

* **bg** : The background color of the bar in the form of a hexadecimal string
* **position** : A string that indicates the edge of the screen the bar should stick to. It can be either **"top"**, **"bottom"**, **"left"**, or **"right"**
* **size** : The thickness of the bar
* **size_activator** : The thickness of the activation region, extending from the edge of the screen. Do note that size of the activator region can not be greater than size of the bar
* **show_delay** : Time in seconds the mouse cursor needs to be within the activation region before the bar slides into view
* **hide_delay** : Time in seconds to wait before the bar slides out of view once the mouse cursor leaves the bar surface
* **easing** : Time in **milliseconds** between each step of the animation
* **delta** : Number of pixels the bar should move between each step of the animation. Presently, if the size of the bar isn't a multiple of the delta value, the bar will overshoot.
* **screen** : In case of a setup with multiple screens, the screen object for the screen the bar should be bound to



To add widgets to the bar, use can the setup method like you would with a regular **wibox**:

```lua
myslidebar:setup {
    layout = wibox.layout.align.horizontal,
    {
        -- {{ Left widget(s) }}
    },
    {
    	-- {{ Middle widget(s) }}       
    },
    { 
        -- {{ Right widgets(s) }}
    },
}
```



So for example if you are using the default **rc.lua** configuration for the current stable release, you will need to do something like this:

```lua
--[[
...
rest of the configuration
...
]] 

local slidebar = require('slidebar')

awful.screen.connect_for_each_screen(function(s)
    --[[
    ...
    setup widgets and stuff
    ...
    ]] 

    -- Create myslidebar instead of 'mywibox'
    s.myslidebar = slidebar {
    	screen = s,
    -- bg = "#282a36",
    -- position = "top",
    -- size = 40,
    -- size_activator = 1
    -- show_delay = 0.25,
    -- hide_delay = 0.5,
    -- easing = 2,
    -- delta = 1,
	}
        
    -- Add widgets to the slidebar
    s.myslidebar:setup {
        layout = wibox.layout.align.horizontal,
        { -- Left widgets
            layout = wibox.layout.fixed.horizontal,
            mylauncher,
            s.mytaglist,
            s.mypromptbox,
        },
        s.mytasklist, -- Middle widget
        { -- Right widgets
            layout = wibox.layout.fixed.horizontal,
            mykeyboardlayout,
            wibox.widget.systray(),
            mytextclock,
            s.mylayoutbox,
        },
    }
end)

--[[
...
rest of the configuration
...
]] 
```



Moreover, you can can tweak the configuration parameters of the bar after it has been instantiated like so:

```lua
-- For example
myslidebar.size = 55
myslidebar.easing = 1
--...

-- Or if accessing the bar from a screen object 's'
s.myslidebar.size = 55
s.myslidebar.easing = 1
```



## License

[![License](http://img.shields.io/:license-mit-blue.svg)](http://doge.mit-license.org)



