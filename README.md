if not game:IsLoaded() then game.Loaded:Wait() end local Players = game:GetService("Players") local RunService = game:GetService("RunService") local UserInputService = game:GetService("UserInputService") local LocalPlayer = Players.LocalPlayer local Camera = workspace.CurrentCamera local Mouse = LocalPlayer:GetMouse()

-- ======= Helpers ======= local function notify(msg) pcall(function() game.StarterGui:SetCore("SendNotification", {Title = "Curtiu o menu? Vem no Discord, keys trade são disponíveis lá! Venha pegar nossos menus pagos através de trades in game!"; Text = tostring(msg); Duration = 15;}) end) end

local function teamCheck(player, onlyEnemies) if not player or not player.Character or not player.Character:FindFirstChild("Humanoid") then return false end if onlyEnemies then return player.Team ~= LocalPlayer.Team else return true end end

-- ======= Rayfield ======= local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({ Name = "StarHub", LoadingTitle = "Star nomad avistado! Carregando módulos de perseguição...", LoadingSubtitle = "@starnomad Edition", ConfigurationSaving = {Enabled = true, FolderName = "PvP_Star_Nomad", FileName = "config"}, KeySystem = false, Theme = "Ocean", })

-- ======= Variables ======= local AimFOV = {Enabled=false, Radius=100, Color=Color3.new(1,0,0), TeamCheck=true, Mode="Keybind", Key=Enum.KeyCode.E} local Aimbot = {Enabled=false, TeamCheck=true, Mode="Keybind", Key=Enum.KeyCode.Q} local ESP = {Enabled=false, Type="Lines", MaxDistance=300, ShowName=true, ShowHealth=true, ShowDistance=true, Color=Color3.new(0,1,0)} local TriggerBot = {Enabled=false, Mode="Keybind", Key=Enum.UserInputType.MouseButton1} local Fly = {Enabled=false, Speed=50, Key=Enum.KeyCode.F} local Noclip = {Enabled=false, Key=Enum.KeyCode.N} local AntiVoid = {Enabled=false} local SpeedHack = {Enabled=false, Multiplier=50, Key=Enum.KeyCode.LeftShift}

-- ======= Tabs ======= local tabScripts = Window:CreateTab("Scripts") local tabSettings = Window:CreateTab("Configurações")

-- ======= Combat Section ======= local sectionCombat = tabScripts:CreateSection("Combate")

-- AimFOV toggle & settings tabScripts:CreateToggle({Name="AimFOV", CurrentValue=false, Flag="aimfov_toggle", Callback=function(val) AimFOV.Enabled = val end}) tabScripts:CreateSlider({Name="AimFOV Radius", Min=50, Max=500, Default=100, Increment=10, Callback=function(val) AimFOV.Radius = val end}) tabScripts:CreateColorPicker({Name="AimFOV Color", Default=Color3.new(1,0,0), Callback=function(col) AimFOV.Color = col end}) tabScripts:CreateDropdown({Name="AimFOV Team Check", Options={"Ignore Team","Only Enemies"}, CurrentOption={"Only Enemies"}, MultipleOptions=false, Callback=function(opt) AimFOV.TeamCheck = (opt[1]=="Only Enemies") end}) tabScripts:CreateDropdown({Name="AimFOV Mode", Options={"Keybind","Toggle"}, CurrentOption={"Keybind"}, MultipleOptions=false, Callback=function(opt) AimFOV.Mode=opt[1] end}) tabScripts:CreateKeybind({Name="AimFOV Keybind", CurrentKeybind=Enum.KeyCode.E, Flag="aimfov_key", Callback=function(key) AimFOV.Key = key end})

-- Aimbot toggle & settings tabScripts:CreateToggle({Name="Aimbot", CurrentValue=false, Flag="aimbot_toggle", Callback=function(val) Aimbot.Enabled = val end}) tabScripts:CreateDropdown({Name="Aimbot Team Check", Options={"Ignore Team","Only Enemies"}, CurrentOption={"Only Enemies"}, MultipleOptions=false, Callback=function(opt) Aimbot.TeamCheck = (opt[1]=="Only Enemies") end}) tabScripts:CreateDropdown({Name="Aimbot Mode", Options={"Keybind","Toggle"}, CurrentOption={"Keybind"}, MultipleOptions=false, Callback=function(opt) Aimbot.Mode=opt[1] end}) tabScripts:CreateKeybind({Name="Aimbot Keybind", CurrentKeybind=Enum.KeyCode.Q, Flag="aimbot_key", Callback=function(key) Aimbot.Key = key end})

-- TriggerBot toggle & settings tabScripts:CreateToggle({Name="TriggerBot", CurrentValue=false, Flag="trigger_toggle", Callback=function(val) TriggerBot.Enabled = val end}) tabScripts:CreateDropdown({Name="TriggerBot Mode", Options={"Keybind","Toggle"}, CurrentOption={"Keybind"}, MultipleOptions=false, Callback=function(opt) TriggerBot.Mode=opt[1] end}) tabScripts:CreateKeybind({Name="TriggerBot Keybind", CurrentKeybind=Enum.UserInputType.MouseButton1, Flag="trigger_key", Callback=function(key) TriggerBot.Key = key end})

-- Fly & Noclip tabScripts:CreateToggle({Name="Fly", CurrentValue=false, Flag="fly_toggle", Callback=function(val) Fly.Enabled = val end}) tabScripts:CreateSlider({Name="Fly Speed", Min=10, Max=250, Default=50, Increment=5, Callback=function(val) Fly.Speed = val end}) tabScripts:CreateKeybind({Name="Fly Keybind", CurrentKeybind=Enum.KeyCode.F, Flag="fly_key", Callback=function(key) Fly.Key = key end}) tabScripts:CreateToggle({Name="Noclip", CurrentValue=false, Flag="noclip_toggle", Callback=function(val) Noclip.Enabled = val end}) tabScripts:CreateKeybind({Name="Noclip Keybind", CurrentKeybind=Enum.KeyCode.N, Flag="noclip_key", Callback=function(key) Noclip.Key = key end})

-- Anti-Void tabScripts:CreateToggle({Name="Anti-Void", CurrentValue=false, Flag="antivoid_toggle", Callback=function(val) AntiVoid.Enabled = val end})

-- SpeedHack tabScripts:CreateToggle({Name="SpeedHack", CurrentValue=false, Flag="speed_toggle", Callback=function(val) SpeedHack.Enabled = val end}) tabScripts:CreateSlider({Name="Speed Multiplier", Min=1, Max=250, Default=50, Increment=5, Callback=function(val) SpeedHack.Multiplier = val end}) tabScripts:CreateKeybind({Name="SpeedHack Keybind", CurrentKeybind=Enum.KeyCode.LeftShift, Flag="speed_key", Callback=function(key) SpeedHack.Key = key end})

-- ESP Section tabScripts:CreateSection("ESP") tabScripts:CreateToggle({Name="ESP", CurrentValue=false, Flag="esp_toggle", Callback=function(val) ESP.Enabled = val end}) tabScripts:CreateDropdown({Name="ESP Type", Options={"Lines","Boxes"}, CurrentOption={"Lines"}, MultipleOptions=false, Callback=function(opt) ESP.Type=opt[1] end}) tabScripts:CreateColorPicker({Name="ESP Color", Default=Color3.new(0,1,0), Callback=function(col) ESP.Color = col end}) tabScripts:CreateSlider({Name="ESP Max Distance", Min=50, Max=1000, Default=300, Increment=50, Callback=function(val) ESP.MaxDistance = val end}) tabScripts:CreateToggle({Name="Show Name", CurrentValue=true, Flag="esp_name", Callback=function(val) ESP.ShowName = val end}) tabScripts:CreateToggle({Name="Show Health", CurrentValue=true, Flag="esp_health", Callback=function(val) ESP.ShowHealth = val end}) tabScripts:CreateToggle({Name="Show Distance", CurrentValue=true, Flag="esp_distance", Callback=function(val) ESP.ShowDistance = val end})

-- ======= Functional Logic ======= local function getClosestPlayer() local closest = nil local shortest = AimFOV.Radius for _, player in pairs(Players:GetPlayers()) do if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then if teamCheck(player, AimFOV.TeamCheck) then local screenPos, onScreen = Camera:WorldToViewportPoint(player.Character.HumanoidRootPart.Position) if onScreen then local dist = (Vector2.new(screenPos.X, screenPos.Y) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude if dist < shortest then closest = player shortest = dist end end end end end return closest end

-- ======= RenderStepped Loop ======= RunService.RenderStepped:Connect(function() -- Aimbot logic if Aimbot.Enabled and ((Aimbot.Mode=="Toggle") or (UserInputService:IsKeyDown(Aimbot.Key))) then local target = getClosestPlayer() if target then local hrp = target.Character:FindFirstChild("HumanoidRootPart") if hrp then Camera.CFrame = CFrame.new(Camera.CFrame.Position, hrp.Position) end end end

-- AimFOV circle drawing (exemplo simples)
-- TriggerBot, ESP, Fly, Noclip, AntiVoid, SpeedHack logic
-- Placeholder: você pode implementar as funções completas aqui usando Rayfield ou Drawing API

end)

notify("A equipe (Guardian Star) não apoia Scripts agressivos ou que invadam sistemas do usuário, tenha segurança e consciência! Não temos a responsabilidade pelo seu banimento (Caso tenha), desde já fique avisado!")

