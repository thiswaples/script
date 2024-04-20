--DELFE'S ULTRA MEGA SCRIPT WOWOWOW

local PlayersService = game:GetService("Players")
local ReplicatedStorageService = game:GetService("ReplicatedStorage")
local LightningService = game:GetService("Lighting")

if not game:IsLoaded() then
	game.Loaded:Wait()
end

print("Delfe's Aura RNG Script -> Game Loaded!")

local LocalPlayer = PlayersService.LocalPlayer

repeat wait() until LocalPlayer.Character

print("Delfe's Aura RNG Script -> Character Loaded!")

local PlayerCharacter = LocalPlayer.Character
local Humanoid = PlayerCharacter.Humanoid
local HumanoidRootPart = PlayerCharacter.HumanoidRootPart

local PlayerGui = LocalPlayer.PlayerGui
local PlayerBackpack = LocalPlayer.Backpack

local PlayerCamera = workspace.Camera

wait(5)

pcall(function()
	local PlayGui = PlayerGui:FindFirstChild("Play")
	PlayGui:Destroy()
end)

pcall(function()
	local BlurLightning = LightningService:FindFirstChild("Blur")
	BlurLightning:Destroy()
end)

PlayerCamera.CameraType = Enum.CameraType.Custom

pcall(function()
	local MainGui = PlayerGui:FindFirstChild("Main")
	MainGui.Enabled = true
end)

local FishingTeleportCFrame = CFrame.new(-413, 40, 52)

function teleport(CF:CFrame)
	HumanoidRootPart.CFrame = CF
end

function equipBestTool()
	Humanoid:UnequipTools()
	local a,b = pcall(function()
		local ChromaticHeart = PlayerBackpack:FindFirstChild("Chromatic Heart")
		Humanoid:EquipTool(ChromaticHeart)
	end)
	if a then
		return
	end
	local a,b = pcall(function()
		local BookOfZen = PlayerBackpack:FindFirstChild("Book Of Zen")
		Humanoid:EquipTool(BookOfZen)
	end)
	if a then
		return
	end
	local a,b = pcall(function()
		local RubyOfDestiny = PlayerBackpack:FindFirstChild("Ruby of Destiny")
		Humanoid:EquipTool(RubyOfDestiny)
	end)
end

-- START AUTO HATCHING & FISHING

teleport(FishingTeleportCFrame)
equipBestTool()



function FindHatchingRemote()
	for i,v in ReplicatedStorageService.Remotes.Hatching:GetDescendants() do
		if v:IsA("RemoteFunction") and #v.Name == 36 then
			return v
		end
	end
end

local HatchRemote = nil

spawn(function()
	while HatchRemote == nil do
		local a,b = pcall(function()
			HatchRemote = FindHatchingRemote()
		end)
		wait(.5)
	end
end)

spawn(function()
	while wait(.1) do
		local a,b = pcall(function()
			if HatchRemote ~= nil then
				HatchRemote:InvokeServer()
			end
		end)
		if not a then
			print(b)
		end
	end
end)

-- START AUTO DUNGEON
spawn(function()
	local DungeonEntranceCFrame = nil
	local DungeonLeaveRemote = nil
	local DungeonChestCFrame = nil
	local DungeonTextUI = nil
	
	local CanDungeon,err = pcall(function()
		local EasyDungeonFolder = workspace.ScriptedMap.Dungeons.Easy
		DungeonEntranceCFrame = EasyDungeonFolder.Door.Main.CFrame
		DungeonTextUI = EasyDungeonFolder.Door.Main.SurfaceGui.Frame.Text
		for i,v in EasyDungeonFolder:GetChildren() do
			for ii,vv in v:GetChildren() do
				if vv.Name == "DUNGEON CHEST 1" then
					DungeonChestCFrame = vv["Plane.001"].CFrame
				end
			end
		end
		DungeonLeaveRemote = ReplicatedStorageService.Remotes.LeftDungeon
		
		wait(5)
		
		while wait(1) do
			if DungeonTextUI.Text == "" then
				teleport(DungeonEntranceCFrame)
				wait(1)
				teleport(DungeonChestCFrame)
				wait(2)
				DungeonLeaveRemote:FireServer()
				wait(2)
				teleport(FishingTeleportCFrame)
				equipBestTool()
			end
		end
	end)
end)







