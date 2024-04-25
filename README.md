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

function EquipHeart()
	Humanoid:UnequipTools()
	wait(0.5)
	local a,b = pcall(function()
		local ChromaticHeart = PlayerBackpack:FindFirstChild("Chromatic Heart")
		Humanoid:EquipTool(ChromaticHeart)
	end)
end

-- START AUTO HATCHING & FISHING

teleport(FishingTeleportCFrame)
wait(0.5)
EquipHeart()

-- START AUTO DUNGEON

spawn(function()
	local DungeonLeaveRemote = nil
	local EasyEntranceCFrame = nil
	local HardEntranceCFrame = nil
	local EasyTimer = nil
	local HardTimer = nil
	
	local CanDungeon,err = pcall(function()
		local EasyDungeonFolder = workspace.ScriptedMap.Dungeons.Easy
		local HardDungeonFolder = workspace.ScriptedMap.Dungeons.Hard
		EasyEntranceCFrame = EasyDungeonFolder.Door.Main.CFrame
		EasyTimer = EasyDungeonFolder.Door.Main.SurfaceGui.Frame.Text
		
		HardEntranceCFrame = HardDungeonFolder.Door.Main.CFrame
		HardTimer = HardDungeonFolder.Door.Main.SurfaceGui.Frame.Text
		DungeonLeaveRemote = ReplicatedStorageService.Remotes.LeftDungeon
		
		wait(5)
		local moved = false
		while wait(1) do
			if EasyTimer.Text == "" then
				teleport(EasyEntranceCFrame)
				wait(2)
				
				pcall(function()
					for i,v in EasyDungeonFolder:GetDescendants() do
						if v.Name == "Plane.001" then
							teleport(v.CFrame)
						end
					end
				end)
				wait(2)
				DungeonLeaveRemote:FireServer()
				wait(2)
				moved = true
			end
			if HardTimer.Text == "" then
				teleport(HardEntranceCFrame)
				
				wait(10)
				
				pcall(function()
					for i,v in HardDungeonFolder:GetDescendants() do
						if v.Name == "Plane.001" then
							teleport(v.CFrame)
						end
					end
				end)
				wait(2)
				DungeonLeaveRemote:FireServer()
				wait(2)
				moved = true
				
			end
			if moved then
				teleport(FishingTeleportCFrame)
				EquipHeart()
				moved = false
			end
		end
	end)
	
	if not CanDungeon then
		print(err)
	end
end)








