local RS = game:GetService("ReplicatedStorage")
local Lightning = game:GetService("Lighting")

local Remotes = nil
local RHatching = nil


local RLoading = nil
local RLD = nil
local SM = nil
local Fishing = nil
local Dungeons = nil
local Easy = nil
local Dungeon = nil
local Door = nil
local DTP = nil
local Chest = nil
local CTP = nil


local SG = nil
local SFG = nil
local TSFG = nil
local Blur = nil
local TP = nil

local player = game.Players.LocalPlayer

local MainGui = nil
local PlayGui = nil

local Camera = nil
local Character = nil
local HRP = nil

wait(10)


function CV()
	if Character==nil then
		Character = player.Character
	end
	if HRP==nil then
		HRP = Character:FindFirstChild("HumanoidRootPart")
	end
	if MainGui==nil then
		MainGui = player.PlayerGui:FindFirstChild("Main")
	end
	if PlayGui==nil then
		PlayGui = player.PlayerGui:FindFirstChild("Play")
	end
	if Camera==nil then
		Camera = workspace.Camera
	end
	if Remotes ==nil then
		Remotes = RS:FindFirstChild("Remotes")
	end
	if RHatching == nil then
		RHatching = Remotes:FindFirstChild("Hatching")
	end
	if RLoading ==nil then
		RLoading = Remotes:FindFirstChild("RLoading")
	end
	if RLD == nil then
		RLD = Remotes:FindFirstChild("LeftDungeon")
	end
	if SM == nil then
		SM = workspace:FindFirstChild("ScriptedMap")
	end
	if Fishing == nil then
		Fishing = SM:FindFirstChild("Fishing")
	end
	if Dungeons ==nil then
		Dungeons = SM:FindFirstChild("Dungeons")
	end
	if Easy == nil then
		Easy = Dungeons:FindFirstChild("Easy")
	end
	if Door == nil then
		Door = Easy:FindFirstChild("Door")
	end
	if DTP == nil then
		DTP = Door:FindFirstChild("Main")
	end
	if SG == nil then
		SG = DTP:FindFirstChild("SurfaceGui")
	end
	if SFG == nil then
		SFG = SG:FindFirstChild("Frame")
	end
	if TSFG == nil then
		TSFG = SFG:FindFirstChild("Text")
	end

	if Blur==nil then
		Blur = Lightning:FindFirstChild("Blur")
	end
	if TP ==nil then
		TP = Fishing:FindFirstChild("Part")
	end


	if Chest == nil then
		for i,v in Easy:GetChildren() do
			for ii,vv in v:GetChildren() do
				if vv.Name=="DUNGEON CHEST 1" then
					Dungeon = v
					Chest = vv
				end
			end
		end
	end
	if CTP == nil then
		CTP = Chest:FindFirstChild("Plane.001")
	end

end


function WFC()
	while wait() do
		local succ,err = pcall(function()
			CV()
		end)
		if succ then
			break
		else
			print(err)
		end
	end
end

while true do
	wait(1)
	if MainGui~=nil and Camera~=nil then
		if PlayGui~=nil and Blur ~=nil then
			PlayGui.Enabled = false
			Blur.Enabled = false
		else
			local succ,err = pcall(function()
				CV()
			end)
			if not succ then
				break
			end
		end
		Camera.CameraType = Enum.CameraType.Custom
		MainGui.Enabled = true
		break
	else
		WFC()
	end
end

spawn(function()
	while task.wait(0.1) do
		local RHatch = RHatching:FindFirstChild("Hatch")
		local RFish = Remotes:FindFirstChild("Fish")
		if RHatch ~=nil then RHatch:InvokeServer() else WFC() end
		if RFish ~=nil then RFish:InvokeServer() else WFC() end
	end
end)


function TPF()
	while wait() do
		if HRP~=nil and TP ~= nil then
			HRP.CFrame = TP.CFrame
			break
		else
			WFC()
		end
	end
end

function TPD()
	while wait() do
		if HRP ~= nil and DTP ~=nil then
			HRP.CFrame = DTP.CFrame
			break
		else
			WFC()
		end
	end
end

function TPC()
	while wait() do
		if HRP ~= nil and CTP ~=nil then
			HRP.CFrame = CTP.CFrame
			break
		else
			WFC()
		end
	end
end

TPF()

while true do	

	if TSFG ~= nil and RLD ~= nil then
		if TSFG.Text=="" then
			spawn(function()
				TPD()
				wait(1)
				TPC()
				wait(3)
				RLD:FireServer()
				wait(3)
				TPF()
			end)
		end
	else
		WFC()
	end
	
	task.wait(5)
end

