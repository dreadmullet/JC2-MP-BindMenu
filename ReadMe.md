### Features

This library has two features: an advanced control system and a bind menu.

The control system allows you to easily process inputs using the events ControlDown, ControlUp, and ControlHeld.

The bind menu gives you a GWEN control that users can set their binds to. Settings are automatically saved to the server database.

Any kind of input is supported:
* Gamepad buttons and axes
* Keys
* Mouse buttons
* Mouse scroll wheel
* Mouse movement

### Usage

#### Creating the bind menu control (client)

```
-- Create the GWEN control.
local bindMenu = BindMenu.Create(parentControl)
-- Add controls, using a name and default input.
bindMenu:AddControl("Toggle left eyelid" ,      "C")                -- Key
bindMenu:AddControl("Toggle right eyelid" ,     "Tab")              -- Key
bindMenu:AddControl("Move eyes left" ,          "Mouse left")       -- Mouse movement
bindMenu:AddControl("Move eyes right" ,         "Mouse right")      -- Mouse movement
bindMenu:AddControl("Cross eyes" ,              "Mouse3")           -- Mouse button
bindMenu:AddControl("Move left eyebrow up" ,    "VehicleFireLeft")  -- Action, used by gamepad button
bindMenu:AddControl("Move left eyebrow down" ,  "MoveLeft")         -- Action, used by gamepad axis
bindMenu:AddControl("Move right eyebrow up" ,   "Mouse wheel up")   -- Mouse wheel
bindMenu:AddControl("Move right eyebrow down" , "Mouse wheel down") -- Mouse wheel
-- Ask the server for our stored settings.
bindMenu:RequestSettings()
```

**Note:** due to a scripting limitation, make sure you call *bindMenu:Remove()* instead of just removing the bind menu's parent.

#### Using controls (client)

You can use the following events:
* ControlDown
* ControlUp
* ControlHeld

Controls have the following properties:
* name (string; Given in the bind menu creation, such as "Toggle left eyelid".)
* type (string)
	* "Action"
	* "Key"
	* "MouseButton"
	* "MouseWheel"
	* "MouseMovement"
* state (number between 0 and 1 for most controls)
	* NOTE: For mouse movement, this is the movement of the mouse in pixels.
* value (used internally)
* valueString (string; The presentable name, such as "Mouse wheel up".)

```
Events:Subscribe("ControlDown" , function(control)
	Chat:Print("Control down: "..control.name , Color(220 , 255 , 220))
end)

Events:Subscribe("ControlUp" , function(control)
	Chat:Print("Control up: "..control.name , Color(255 , 220 , 220))
end)

Events:Subscribe("ControlHeld" , function(control)
	Chat:Print("Control held: "..control.name..", state: "..control.state , Color(255 , 220 , 220))
end)
```

**Be careful when using mouse movement's state.** If you have a boost button that multiplies the boost by *control.state*, be sure to clamp it between 0 and 1. Otherwise, if someone binds a mouse direction to it, they can easily boost 20 times as much as they should, because *control.state* is the number of pixels moved per frame.

#### Other functions

* Controls.Add(name, default)
* Controls.Remove(controlName)
* Controls.GetInputNameByControl(controlName)
	* Example: *Controls.GetInputNameByControl("Cross eyes")* will return "Mouse3"
* Controls.Get(controlName)
* Controls.GetIsHeld(controlName)

### Examples

See https://github.com/dreadmullet/JC2-MP-Noclip for a full demonstration of how to use controls and set up the bind menu.

### Screenshots

Noclip's settings menu.

![alt tag](http://i.imgur.com/hEcUIAW.jpg)

Racing's setting menu.

![alt tag](http://i.imgur.com/Glpb6Qb.jpg)
