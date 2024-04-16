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
	pcall(function()
		local ChromaticHeart = PlayerBackpack:FindFirstChild("Chromatic Heart")
		Humanoid:EquipTool(ChromaticHeart)
	end)
	pcall(function()
		local BookOfZen = PlayerBackpack:FindFirstChild("Book Of Zent")
		Humanoid:EquipTool(BookOfZen)
	end)
	pcall(function()
		local RubyOfDestiny = PlayerBackpack:FindFirstChild("Ruby of Destiny")
		Humanoid:EquipTool(RubyOfDestiny)
	end)
end

-- START AUTO HATCHING & FISHING
spawn(function()
	teleport(FishingTeleportCFrame)
	equipBestTool()
	
	local HatchingRemote = nil
	while wait() do
		local CanAutoHatch,err1 = pcall(function()
			HatchingRemote = ReplicatedStorageService.Remotes.Hatching.Hatch
		end)
		if CanAutoHatch then
			break
		end
	end
	local FishingRemote = nil
	while wait() do
		local CanAutoFishing,err2 = pcall(function()
			FishingRemote = ReplicatedStorageService.Remotes.Fish
		end)
		if CanAutoFishing then
			break
		end
	end
	
	
	
	spawn(function()
		print("Hatching..")
		while wait(.1) do
			HatchingRemote:InvokeServer()
		end
	end)

	spawn(function()
		print("Fishing..")
		while wait(.1) do
			FishingRemote:InvokeServer()
		end
	end)
	
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
	end)
	
	wait(5)
	
	if CanDungeon then
		spawn(function()
			while wait(1) do
				if DungeonTextUI.Text == "" then
					teleport(DungeonEntranceCFrame)
					wait(2)
					teleport(DungeonChestCFrame)
					wait(3)
					DungeonLeaveRemote:FireServer()
					wait(3)
					teleport(FishingTeleportCFrame)
					equipBestTool()
				end
			end
		end)
	else
		print(err)
	end
end)







