getgenv().Prediction =  (  0  )   -- [ PREDICTION: THIS IS ANARCHY IT DOESNT REQUIRE ANYTHING MORE THAN .01  ]

getgenv().FOV =  (  30  )   -- [ FOV RADIUS: Increases Or Decreases FOV Radius ]

getgenv().AimKey =  (  "["  )  -- [ TOGGLE KEY: Toggles Silent Aim On And Off ]

--[[
		Do Not Edit Anything Beyond This Point. 
]]

local SilentAim = true
local NeckOffSet = Vector3.new(0,-.5,0)
local Players = game:GetService("Players")
local LocalPlayer = game:GetService("Players").LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Camera = game:GetService("Workspace").CurrentCamera
local connections = getconnections(game:GetService("LogService").MessageOut)
for _, v in ipairs(connections) do
	v:Disable()
end

getgenv = getgenv
Drawing = Drawing
getrawmetatable = getrawmetatable
getconnections = getconnections
hookmetamethod = hookmetamethod

local FOV_CIRCLE = Drawing.new("Circle")
FOV_CIRCLE.Visible = true
FOV_CIRCLE.Filled = false
FOV_CIRCLE.Thickness = 1
FOV_CIRCLE.Transparency = .4
FOV_CIRCLE.Color = Color3.new(0, 1, 0)
FOV_CIRCLE.Radius = getgenv().FOV
FOV_CIRCLE.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)

local Options = {
	Torso = "HumanoidRootPart";
	Head = "Head";
    Torso = "LowerTorso";
    Torso = "UpperTorso";
}

local function MoveFovCircle()
	pcall(function()
		local DoIt = true
		spawn(function()
			while DoIt do task.wait()
				FOV_CIRCLE.Position = Vector2.new(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y)
			end
		end)
	end)
end coroutine.wrap(MoveFovCircle)()

Mouse.KeyDown:Connect(function(KeyPressed)
	if KeyPressed == (getgenv().AimKey:lower()) then
		if SilentAim == false then
			FOV_CIRCLE.Color = Color3.new(0, 1, 0)
			SilentAim = true
		elseif SilentAim == true then
			FOV_CIRCLE.Color = Color3.new(1, 0, 0)
			SilentAim = false
		end
	end
end)
Mouse.KeyDown:Connect(function(Rejoin)
	if Rejoin == "=" then
		local LocalPlayer = game:GetService("Players").LocalPlayer
		game:GetService("TeleportService"):Teleport(game.PlaceId, LocalPlayer)
	end
end)

getgenv().FORCE_REPEAT = {
  "FORIINDEXRETURNADMININGAME";
	"AimLockPsycho";
	"proboy32007";
	"autofarmaccountgrind";
}

local function InRadius()
	local Target = nil
	local Distance = 9e9
	local Players = game:GetService("Players")
	local LocalPlayer = game:GetService("Players").LocalPlayer
	local Camera = game:GetService("Workspace").CurrentCamera
	for _, v in pairs(Players:GetPlayers()) do 
		if not table.find(getgenv().FORCE_REPEAT, v.Name) then
			if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") and v.Character:FindFirstChild("Humanoid") and v.Character:FindFirstChild("Humanoid").Health > 0 then
				local Enemy = v.Character	
				local CastingFrom = CFrame.new(Camera.CFrame.Position, Enemy[Options.Torso].CFrame.Position) * CFrame.new(0, 0, -4)
				local RayCast = Ray.new(CastingFrom.Position, CastingFrom.LookVector * 9000)
				local World, ToSpace = game:GetService("Workspace"):FindPartOnRayWithIgnoreList(RayCast, {LocalPlayer.Character:FindFirstChild("Head")})
				local RootWorld = (Enemy[Options.Torso].CFrame.Position - ToSpace).magnitude
				if RootWorld < 4 then		
					local RootPartPosition, Visible = Camera:WorldToViewportPoint(Enemy[Options.Torso].Position)
					if Visible then
						local Real_Magnitude = (Vector2.new(Mouse.X, Mouse.Y) - Vector2.new(RootPartPosition.X, RootPartPosition.Y)).Magnitude
						if Real_Magnitude < Distance and Real_Magnitude < FOV_CIRCLE.Radius then
							Distance = Real_Magnitude
							Target = Enemy
						end
					end
				end
			end
		end
	end
	return Target
end

local oldIndex = nil
oldIndex = hookmetamethod(game, "__index", function(self, Index, Screw)
	local Screw = oldIndex(self, Index)
	local CALC = Mouse
	local GG = "hit"
	local MONCLER = GG
    if SilentAim then
	if self == CALC and (Index:lower() == MONCLER) then	
		local Enemy = InRadius()
		local Camera = game:GetService("Workspace").CurrentCamera
		if Enemy ~= nil and Enemy[Options.Head] and Enemy:FindFirstChild("Humanoid") and Enemy:FindFirstChild("Humanoid").Health > 0 then
			local Madox = Enemy[Options.Head]
			local Formulate = Madox.CFrame + (Madox.AssemblyLinearVelocity * getgenv().Prediction + NeckOffSet)	
			return (Index:lower() == MONCLER and Formulate)
		end
		return Screw
	end
    end
	return oldIndex(self, Index, Screw)
end)

--Farewell Atman, Nosss, Toru.
