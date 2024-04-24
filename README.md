


function CheckLevel()
    local MyLevel = game.Players.LocalPlayer.Data.Levels.Value
    if MyLevel == 1 or plrlvl <= 200 then
        MON = "BaconHair"
    elseif MyLevel >= 101 or MyLevel <= 300 then
        MON = "Bacon Buggy"
    elseif MyLevel >= 301 or MyLevel <= 500 then
        MON = "Bancon Big"
    elseif MyLevel >= 501 or MyLevel <= 1500 then
        MON = "Bacon King"
    elseif MyLevel >= 1501 or MyLevel <= 2500 then
        MON = "Boss Snow"
    elseif MyLevel >= 1501 or MyLevel <= 5000 then
        MON = "Bacon Real"
    end
    
end

function CheckHealth()
    local plr = game.Players.LocalPlayer.Humanoid
    if plr.Health == 0 or plr.Health <= 50 then
        _G.AutoKaiton = false
        wait(.1)
        game.Players.LocalPlayer.HumanoidRootPart.CFrame = CFrame.new(0,300,0)       
    elseif plr.Health >= 50 then
        _G.AutoKaiton = true 
    end
end

--[[function TP(P)
    local Distance = (P.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude -- จุดที่จะไป Position Only
    local Speed = 300 -- ความเร็วของมึง
    tweenService, tweenInfo = game:GetService("TweenService"), TweenInfo.new(Distance/Speed, Enum.EasingStyle.Linear)
    tween = tweenService:Create(game:GetService("Players")["LocalPlayer"].Character.HumanoidRootPart, tweenInfo, {CFrame = P})
    tween:Play()
end]]

if _G.AutoKaiton == true then
    CheckHealth()
    local Noclip = Instance.new("Part",Workspace) 
    Noclip.Name = "Thepartformscript" 
    Noclip.CanCollide = true 
    Noclip.Anchored = true 
    Noclip.Transparency = 0.5 
    Noclip.Size = Vector3.new(10,-3,10) 
    Noclip.CFrame = Vector3.new(0,300,0)
end

spawn(function()
    game:GetService("RunService").RenderStepped:Connect(function()
     pcall(function()
        if _G.AutoKaiton then
            repeat wait()
            CheckHealth()
            CheckLevel()
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = game:GetService("Workspace")["NPC DAMAGE"][MON].HumanoidRootPart.CFrame * CFrame.new(0,16,0)
                until _G.AutoKaiton == false 
            end
        end)
    end)
end)

spawn(function()
    pcall(function()
        if _G.AutoKaiton == true then
            _G.HideHB = true -- true = hide / false = 0.5
            _G.Hitbox = true
        for i,v in pairs(game:GetService("Workspace")["NPC DAMAGE"]:GetChildren()) do
            v.HumanoidRootPart.Size = Vector3.new(26, 26, 26)
            v.HumanoidRootPart.CanCollide = false
            if _G.HideHB == true then
                v.HumanoidRootPart.Transparency = 1
            elseif _G.HideHB == false then
                v.HumanoidRootPart.Transparency = 0.5
            end
            end
        end
    end)
end)

spawn(function()
    game:GetService("RunService").RenderStepped:Connect(function()
     pcall(function()
         if _G.AutoKaiton then
             game:GetService'VirtualUser':CaptureController()
             game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
         end
     end)
 end) 
 end)

local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()

local Window = Library.CreateLib("Viet nam piece", "DarkTheme")

local Tab = Window:NewTab("Kaiton")
local Section = Tab:NewSection("Kaiton")
Section:NewButton("Start auto kaiton", "", function()
    _G.AutoKaiton = true
end)
Section:NewButton("Stops auto kaiton", "", function()
    _G.AutoKaiton = false
end)

Section:NewButton("Auto stat Sword", "", function()
    _G.AutoSword = true;
    while _G.AutoSword do wait(.1)
        local args = {
            [1] = "Sword",
            [2] = 1
        }
        
        game:GetService("ReplicatedStorage"):WaitForChild("StatSystem"):WaitForChild("Points"):FireServer(unpack(args))
    end
end)



local Tab = Window:NewTab("Auto Equip")
local Section = Tab:NewSection("Auto Equip")
local Weaponlist = {}
local Weapon = nil
for i,v in pairs(game:GetService("Players").LocalPlayer.Backpack:GetChildren()) do
    table.insert(Weaponlist,v.Name)
end
Section:NewDropdown("select weapon", " ", Weaponlist, function(currentOption)
    Weapon = currentOption
end)
Section:NewToggle("Auto Equip", " ", function(a)
AutoEquiped = a
end)
spawn(function()
while wait() do
if AutoEquiped then
pcall(function()
game.Players.LocalPlayer.Character.Humanoid:EquipTool(game:GetService("Players").LocalPlayer.Backpack:FindFirstChild(Weapon))
end)
end
end
end)

