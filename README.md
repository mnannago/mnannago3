local Effects_Enabled = true
local FallingDamage_Power = 15



local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
local UserInputService = game:GetService("UserInputService")
Player.CharacterAdded:Connect(function(Char)
	Character = Char
	Humanoid = Char:WaitForChild("Humanoid")
	HumanoidRootPart = Char:WaitForChild("HumanoidRootPart")
end)

local Run = game:GetService("RunService")
local Camera = game.Workspace.CurrentCamera
local Gyro = Instance.new("BodyGyro",HumanoidRootPart)
Gyro.Name = "SkydiveGyro"
local IsFalling = Instance.new("BoolValue",HumanoidRootPart)
IsFalling.Name = "IsFalling"
IsFalling.Value = false
local MainAnimation
local ForwardAnimation
local RightAnimation
local LeftAnimation
local ContrailBodyPart1
local ContrailBodyPart2
local ContrailBodyPart3
local ContrailBodyPart4
local AttachmentPosition1 = Vector3.new(-0.3,0,0)
local AttachmentPosition2 = Vector3.new(0.3,0,0)
local AtmosphericPressureInfluence = true
if Humanoid.RigType == Enum.HumanoidRigType.R6 then
	MainAnimation = script:WaitForChild("MainAnimation").AnimationId
	ForwardAnimation = script.MainAnimation.AnimationId
	RightAnimation = script.MainAnimation.AnimationId
	LeftAnimation = script.MainAnimation.AnimationId
	ContrailBodyPart1 = Character:WaitForChild("Left Arm")
	ContrailBodyPart2 = Character:WaitForChild("Right Arm")
	ContrailBodyPart3 = Character:WaitForChild("Left Leg")
	ContrailBodyPart4 = Character:WaitForChild("Right Leg")
	AttachmentPosition1 = Vector3.new(-0.3,-0.9,0)
	AttachmentPosition2 = Vector3.new(0.3,-0.9,0)
end

local Animation = Instance.new("Animation",Character)
Animation.AnimationId = MainAnimation
local FreeFallAnimation = Humanoid:LoadAnimation(Animation)
FreeFallAnimation.Priority = Enum.AnimationPriority.Core

local Animation2 = Instance.new("Animation",Character)
Animation2.AnimationId = ForwardAnimation
local FreeFallAnimationForward = Humanoid:LoadAnimation(Animation2)
	
local Animation3 = Instance.new("Animation",Character)
Animation3.AnimationId = LeftAnimation
local FreeFallAnimationLeft = Humanoid:LoadAnimation(Animation3)
FreeFallAnimationLeft.Priority = Enum.AnimationPriority.Idle
	
local Animation4 = Instance.new("Animation",Character)
Animation4.AnimationId = RightAnimation
local FreeFallAnimationRight = Humanoid:LoadAnimation(Animation4)
FreeFallAnimationRight.Priority = Enum.AnimationPriority.Idle


local InitialGravity = workspace.Gravity
local DragRatio = 0.997
local DragDiminish = 0
local Drag = DragRatio
local MinimumSpeed = -91.1344415
local GlideLR = 0
local GlideFB = 0
local TiltLR = 0
local TiltFB = 0
local MinimumAltitude = math.huge

if Effects_Enabled == true then
	local Attachment1A = Instance.new("Attachment",ContrailBodyPart1)
	Attachment1A.Position = AttachmentPosition1
	local Attachment2A = Instance.new("Attachment",ContrailBodyPart1)
	Attachment2A.Position = AttachmentPosition2
	ContrailsA = Instance.new("Trail",ContrailBodyPart1)
	ContrailsA.Attachment0 = Attachment1A
	ContrailsA.Attachment1 = Attachment2A
	ContrailsA.Texture = "rbxassetid://3517446796"
	ContrailsA.LightInfluence = 1
	ContrailsA.TextureLength = 1
	ContrailsA.FaceCamera = true
	ContrailsA.Transparency = NumberSequence.new{
		NumberSequenceKeypoint.new(0,1),
		NumberSequenceKeypoint.new(0.1,0.7),
		NumberSequenceKeypoint.new(1,1)
	}
	local Attachment1B = Instance.new("Attachment",ContrailBodyPart2)
	Attachment1B.Position = AttachmentPosition1
	local Attachment2B = Instance.new("Attachment",ContrailBodyPart2)
	Attachment2B.Position = AttachmentPosition2
	ContrailsB = Instance.new("Trail",ContrailBodyPart2)
	ContrailsB.Attachment0 = Attachment1B
	ContrailsB.Attachment1 = Attachment2B
	ContrailsB.Texture = "rbxassetid://3517446796"
	ContrailsB.LightInfluence = 1
	ContrailsB.TextureLength = 1
	ContrailsB.FaceCamera = true
	ContrailsB.Transparency = NumberSequence.new{
		NumberSequenceKeypoint.new(0,1),
		NumberSequenceKeypoint.new(0.1,0.7),
		NumberSequenceKeypoint.new(1,1)
	}
	local Attachment1C = Instance.new("Attachment",ContrailBodyPart3)
	Attachment1C.Position = AttachmentPosition1
	local Attachment2C = Instance.new("Attachment",ContrailBodyPart3)
	Attachment2C.Position = AttachmentPosition2
	ContrailsC = Instance.new("Trail",ContrailBodyPart3)
	ContrailsC.Attachment0 = Attachment1C
	ContrailsC.Attachment1 = Attachment2C
	ContrailsC.Texture = "rbxassetid://3517446796"
	ContrailsC.LightInfluence = 1
	ContrailsC.TextureLength = 1
	ContrailsC.FaceCamera = true
	ContrailsC.Transparency = NumberSequence.new{
		NumberSequenceKeypoint.new(0,1),
		NumberSequenceKeypoint.new(0.1,0.7),
		NumberSequenceKeypoint.new(1,1)
	}
	local Attachment1D = Instance.new("Attachment",ContrailBodyPart4)
	Attachment1D.Position = AttachmentPosition1
	local Attachment2D = Instance.new("Attachment",ContrailBodyPart4)
	Attachment2D.Position = AttachmentPosition2
	ContrailsD = Instance.new("Trail",ContrailBodyPart4)
	ContrailsD.Attachment0 = Attachment1D
	ContrailsD.Attachment1 = Attachment2D
	ContrailsD.Texture = "rbxassetid://3517446796"
	ContrailsD.LightInfluence = 1
	ContrailsD.TextureLength = 1
	ContrailsD.FaceCamera = true
	ContrailsD.Transparency = NumberSequence.new{
		NumberSequenceKeypoint.new(0,1),
		NumberSequenceKeypoint.new(0.1,0.7),
		NumberSequenceKeypoint.new(1,1)
	}
end

local function CreatW()
	TiltFB = -1
	GlideFB = 2
	Drag = DragRatio*(0.997+DragDiminish)
	if IsFalling.Value == true then
		FreeFallAnimationForward:Play(0.25)
	end
end

local function CreatA()
	TiltLR = 1
	GlideLR = -1
	if IsFalling.Value == true then
		FreeFallAnimationLeft:Play(0.25)
	end
end

local function CreatD()
	TiltLR = -1
	GlideLR = 1
	if IsFalling.Value == true then
		FreeFallAnimationRight:Play(0.25)
	end 
end

local function StopAll()
	TiltLR = 0
	GlideLR = 0
	FreeFallAnimationLeft:Stop()
	FreeFallAnimationRight:Stop()
end

Player:GetMouse().KeyDown:Connect(function(Key)
	if Key:byte() == 119 or Key:byte() == 17 then 
		CreatW()
		
	elseif Key:byte() == 97 or Key:byte() == 20 then 
		CreatA()
	elseif Key:byte() == 100 or Key:byte() == 19 then 
		CreatD()
	elseif (Key:byte() == 97 and Key:byte() == 100) or (Key:byte() == 20 and Key:byte() == 19) then
		StopAll()
    end
end)

local MobileGui = script:WaitForChild("MobileControl")


if UserInputService.TouchEnabled and not UserInputService.KeyboardEnabled then
	MobileGui.Parent = Player.PlayerGui
	local WFrame = MobileGui.AFrame
	local AFrame = MobileGui.WFrame
	local DFrame = MobileGui.DFrame

	WFrame.MouseEnter:Connect(function()
		CreatW()
	end)
	WFrame.MouseLeave:Connect(function()
		StopW()
	end)

	AFrame.MouseEnter:Connect(function()
		CreatA()
	end)
	AFrame.MouseLeave:Connect(function()
		StopAD()
	end)

	DFrame.MouseEnter:Connect(function()
		CreatD()
	end)
	DFrame.MouseLeave:Connect(function()
		StopAD()
	end)
end



function StopW()
	TiltFB = 0
	GlideFB = 0
	Drag = DragRatio
	FreeFallAnimationForward:Stop()
end

function StopAD()
	TiltLR = 0
	GlideLR = 0
	FreeFallAnimationLeft:Stop()
	FreeFallAnimationRight:Stop()
end




Player:GetMouse().KeyUp:Connect(function(Key)
    if Key:byte() == 119 or Key:byte() == 17 then 
		StopW()
	elseif (Key:byte() == 97 or Key:byte() == 20) or (Key:byte() == 100 or Key:byte() == 19) then 
		StopAD()
	end
end)


IsFalling.Changed:Connect(function()
	if IsFalling.Value == true then
		FreeFallAnimation:Play(1)
	else
		FreeFallAnimation:Stop()
		FreeFallAnimationForward:Stop()
		FreeFallAnimationLeft:Stop()
		FreeFallAnimationRight:Stop()
	end
end)


Velocity = 0
local function FreeFalling()
	while true do
		local x = wait()
		if script.Parent.Torso.Velocity.Y <= -10 then 
			Velocity = Velocity + x
		end
		if script.Parent.Torso.Velocity.Y > -10 then 
			if Velocity >= 1 then 
				if not script.Parent:FindFirstChild("Parachute") then
					script.Parent.Humanoid:TakeDamage(Velocity * FallingDamage_Power) 
				end

			end
			Velocity = 0
		end
	end
end

spawn(FreeFalling)


Run.RenderStepped:Connect(function()
	local Speed = HumanoidRootPart.Velocity.Y
	local lookVector = Camera.CFrame.LookVector
	
	if AtmosphericPressureInfluence == false then
		DragDiminish = 0
		MinimumAltitude = math.huge
	else
		DragDiminish = -((101.325*((((((86)-(0.00356*(HumanoidRootPart.Position.Y))+459.67)/(459.67+(86)))^5.256))))/101.325)*0.0015+0.0015
		MinimumAltitude = (160000/(1+1.0001^HumanoidRootPart.Velocity.Y))-15000 
	end

	if Speed <= MinimumSpeed and Humanoid.Sit == false and HumanoidRootPart.Position.Y <= MinimumAltitude and Humanoid.Health > 0 then
		workspace.Gravity = 32.174  
		HumanoidRootPart.Velocity = HumanoidRootPart.Velocity*(Drag+DragDiminish)+Vector3.new(GlideLR,1,GlideLR)*HumanoidRootPart.CFrame.RightVector+Vector3.new(GlideFB,1,GlideFB)*HumanoidRootPart.CFrame.LookVector
		Gyro.MaxTorque = Vector3.new(40000000,40000000,40000000)
		Gyro.CFrame = CFrame.new(HumanoidRootPart.Position)*CFrame.Angles(0,math.atan2(-lookVector.X,-lookVector.Z),TiltLR/2)*CFrame.Angles(TiltFB,0,0)
		Humanoid.PlatformStand = true
		IsFalling.Value = true
		Character.Animate.Disabled = true
		pcall(function()
			ContrailsA.Enabled = true
			ContrailsB.Enabled = true
			ContrailsC.Enabled = true
			ContrailsD.Enabled = true
		end)	
	else
		workspace.Gravity = InitialGravity
		Gyro.MaxTorque = Vector3.new(0,0,0)
		Humanoid.PlatformStand = false
		IsFalling.Value = false
		Character.Animate.Disabled = false
		pcall(function()
			ContrailsA.Enabled = false
			ContrailsB.Enabled = false
			ContrailsC.Enabled = false
			ContrailsD.Enabled = false
		end)	
	end
end)

