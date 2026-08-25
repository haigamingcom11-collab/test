test
-- ALB HUB V3 - Optimized for Delta X & Mobile (RGB LED & Enhanced Features)
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")

local Window = Rayfield:CreateWindow({
   Name = "⚡ ALB HUB V3 | ah tí dz V3 ⚡",
   LoadingTitle = "ALB HUB Loading...",
   LoadingSubtitle = "by ah tí dz",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "ALBHubConfig",
      FileName = "ALBHub"
   },
   Discord = { Enabled = false },
   KeySystem = false
})

-- UI Styling & Persistence
local CoreGui = game:GetService("CoreGui")
Rayfield:Notify({
   Title = "⚡ ALB HUB V3 Loaded",
   Content = "Đã cập nhật FOV Cố Định Trung Tâm, Aim Max 999 & Bỏ Qua Xác Chết!",
   Duration = 6.5,
   Image = "rbxassetid://4483362458",
})

-- FPS Counter & Animated LED Outline
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = CoreGui
ScreenGui.Name = "ALB_FPS_Gui"

local FPSLabel = Instance.new("TextLabel")
FPSLabel.Parent = ScreenGui
FPSLabel.Size = UDim2.new(0, 110, 0, 32)
FPSLabel.Position = UDim2.new(1, -120, 0, 10)
FPSLabel.BackgroundTransparency = 0.3
FPSLabel.BackgroundColor3 = Color3.fromRGB(15, 10, 25)
FPSLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
FPSLabel.TextStrokeTransparency = 0.5
FPSLabel.TextSize = 14
FPSLabel.Font = Enum.Font.GothamBold

local Corner = Instance.new("UICorner", FPSLabel)
Corner.CornerRadius = UDim.new(0, 8)

local Stroke = Instance.new("UIStroke", FPSLabel)
Stroke.Thickness = 2

-- Dynamic RGB LED Loop
task.spawn(function()
    local hue = 0
    while task.wait() do
        hue = (hue + 0.005) % 1
        Stroke.Color = Color3.fromHSV(hue, 1, 1)
    end
end)

local LastTime = tick()
local FrameCount = 0
RunService.RenderStepped:Connect(function()
    FrameCount = FrameCount + 1
    if tick() - LastTime >= 1 then
        FPSLabel.Text = "⚡ FPS: " .. tostring(FrameCount)
        FrameCount = 0
        LastTime = tick()
    end
end)

-- Variables & Toggles State
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local Mouse = LocalPlayer:GetMouse()

local Settings = {
    PlayerESP = false,
    ItemESP = false,
    Aimbot = false,
    AimTarget = "Head",
    AimSmoothness = 0.5, -- Mặc định 0.5
    HitboxSize = 2,
    HitboxExpanded = false,
    NoClip = false,
    XRay = false,
    Speed = 16,
    SpeedEnabled = false,
    Gravity = 196.2,
    GravEnabled = false,
    AirWalk = false,
    MagnetItems = false,
    ClickTP = false,
    SelectedPlayer = nil
}

-- TABS WITH ICONS
local TabMain = Window:CreateTab("Trang Chính", "home")
local TabESP = Window:CreateTab("Định Vị (ESP)", "eye")
local TabCombat = Window:CreateTab("Tấn Công", "swords")
local TabMovement = Window:CreateTab("Di Chuyển", "zap")
local TabVisuals = Window:CreateTab("Đồ Họa & Sv", "monitor")
local TabScripts = Window:CreateTab("Scripts Khác", "code")

-- 1. TAB MAIN
TabMain:CreateButton({
   Name = "👻 Tàng Hình (Invisible)",
   Callback = function()
      loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Awesome-Invisible-man-21074"))()
   end,
})

TabMain:CreateButton({
   Name = "🏠 Teleport Về Spawn / Nhà",
   Callback = function()
      if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
          local spawnLocation = workspace:FindFirstChildOfClass("SpawnLocation")
          if spawnLocation then
              LocalPlayer.Character.HumanoidRootPart.CFrame = spawnLocation.CFrame + Vector3.new(0, 5, 0)
          end
      end
   end,
})

TabMain:CreateToggle({
   Name = "🛡️ Anti-AFK (Treo Game Không Văng)",
   CurrentValue = true,
   Callback = function(Value)
      local VirtualUser = game:GetService("VirtualUser")
      LocalPlayer.Idled:Connect(function()
          if Value then
              VirtualUser:CaptureController()
              VirtualUser:ClickButton2(Vector2.new())
          end
      end)
   end,
})

-- 2. TAB MOVEMENT
TabMovement:CreateToggle({
   Name = "🚀 Bật Tăng Tốc Độ (Speed)",
   CurrentValue = false,
   Callback = function(Value)
      Settings.SpeedEnabled = Value
      task.spawn(function()
          while Settings.SpeedEnabled do
              if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
                  LocalPlayer.Character.Humanoid.WalkSpeed = Settings.Speed
              end
              task.wait(0.1)
          end
          if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
              LocalPlayer.Character.Humanoid.WalkSpeed = 16
          end
      end)
   end,
})

TabMovement:CreateSlider({
   Name = "⚡ Chỉnh Tốc Độ (Speed)",
   Range = {1, 500},
   Increment = 1,
   Suffix = " Speed",
   CurrentValue = 16,
   Callback = function(Value)
      Settings.Speed = Value
   end,
})

TabMovement:CreateToggle({
   Name = "🌐 Bật Trọng Lực (Gravity)",
   CurrentValue = false,
   Callback = function(Value)
      Settings.GravEnabled = Value
      if not Value then workspace.Gravity = 196.2 end
      task.spawn(function()
          while Settings.GravEnabled do
              workspace.Gravity = Settings.Gravity
              task.wait(0.1)
          end
      end)
   end,
})

TabMovement:CreateSlider({
   Name = "🪐 Chỉnh Trọng Lực (Gravity)",
   Range = {0, 500},
   Increment = 1,
   Suffix = " Grav",
   CurrentValue = 196,
   Callback = function(Value)
      Settings.Gravity = Value
   end,
})

TabMovement:CreateToggle({
   Name = "👻 Đi Xuyên Tường (NoClip)",
   CurrentValue = false,
   Callback = function(Value)
      Settings.NoClip = Value
      RunService.Stepped:Connect(function()
          if Settings.NoClip and LocalPlayer.Character then
              for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
                  if v:IsA("BasePart") then v.CanCollide = false end
              end
          end
      end)
   end,
})

-- AirWalk
local Platform = Instance.new("Part")
Platform.Size = Vector3.new(6, 1, 6)
Platform.Anchored = true
Platform.Transparency = 1

TabMovement:CreateToggle({
   Name = "🦘 Nhảy Vô Hạn / Đứng Trên Không (AirWalk)",
   CurrentValue = false,
   Callback = function(Value)
      Settings.AirWalk = Value
      task.spawn(function()
          while Settings.AirWalk do
              if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                  Platform.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame - Vector3.new(0, 3.5, 0)
                  Platform.Parent = workspace
              end
              task.wait()
          end
          Platform.Parent = nil
      end)
   end,
})

-- Click TP
TabMovement:CreateToggle({
   Name = "📍 Click To Teleport (Click TP)",
   CurrentValue = false,
   Callback = function(Value)
      Settings.ClickTP = Value
   end,
})

Mouse.Button1Down:Connect(function()
    if Settings.ClickTP and Mouse.Hit then
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
            LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(Mouse.Hit.Position + Vector3.new(0, 3, 0))
        end
    end
end)

-- Teleport To Player Section
local PlayerDropdown = TabMovement:CreateDropdown({
   Name = "👤 Chọn Người Chơi Để TP",
   Options = {"Đang tải..."},
   CurrentOption = "",
   Callback = function(Option)
      Settings.SelectedPlayer = Option[1]
   end,
})

local function UpdatePlayerList()
    local plist = {}
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer then table.insert(plist, p.Name) end
    end
    PlayerDropdown:Refresh(plist)
end
Players.PlayerAdded:Connect(UpdatePlayerList)
Players.PlayerRemoving:Connect(UpdatePlayerList)
task.spawn(UpdatePlayerList)

TabMovement:CreateButton({
   Name = "⚡ TP Ra Sau Lưng Người Chơi Đã Chọn",
   Callback = function()
      if Settings.SelectedPlayer then
          local targetPlr = Players:FindFirstChild(Settings.SelectedPlayer)
          if targetPlr and targetPlr.Character and targetPlr.Character:FindFirstChild("HumanoidRootPart") then
              local targetHRP = targetPlr.Character.HumanoidRootPart
              if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                  LocalPlayer.Character.HumanoidRootPart.CFrame = targetHRP.CFrame * CFrame.new(0, 0, 3)
              end
          end
      end
   end,
})

-- 3. TAB COMBAT & HITBOX
TabCombat:CreateToggle({
   Name = "📦 Mở Rộng Hitbox (Màu Tím)",
   CurrentValue = false,
   Callback = function(Value)
      Settings.HitboxExpanded = Value
      task.spawn(function()
          while Settings.HitboxExpanded do
              for _, plr in pairs(Players:GetPlayers()) do
                  if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
                      local hrp = plr.Character.HumanoidRootPart
                      hrp.Size = Vector3.new(Settings.HitboxSize, Settings.HitboxSize, Settings.HitboxSize)
                      hrp.Transparency = 0.7
                      hrp.Color = Color3.fromRGB(180, 0, 255)
                      hrp.Material = Enum.Material.Neon
                      hrp.CanCollide = false
                  end
              end
              task.wait(0.5)
          end
      end)
   end,
})

TabCombat:CreateSlider({
   Name = "📐 Kích Thước Hitbox",
   Range = {2, 200},
   Increment = 1,
   Suffix = " Size",
   CurrentValue = 2,
   Callback = function(Value)
      Settings.HitboxSize = Value
   end,
})

-- Improved Smart Aimbot (Center Screen FOV + Alive Check + Smooth Max 999)
local FOVCircle = Drawing.new("Circle")
FOVCircle.Color = Color3.fromRGB(180, 0, 255)
FOVCircle.Thickness = 1.5
FOVCircle.Radius = 120
FOVCircle.Visible = false
FOVCircle.NumSides = 64

TabCombat:CreateToggle({
   Name = "🎯 Aimbot Thông Minh (Không Xuyên Tường)",
   CurrentValue = false,
   Callback = function(Value)
      Settings.Aimbot = Value
      FOVCircle.Visible = Value
   end,
})

TabCombat:CreateSlider({
   Name = "🧲 Độ Dính / Khóa Mục Tiêu (Aimbot Smooth)",
   Range = {1, 999},
   Increment = 1,
   Suffix = " Level",
   CurrentValue = 500,
   Callback = function(Value)
      -- Chuyển slider từ 1 -> 999 thành tỷ lệ mượt (0.01 -> 1.0)
      Settings.AimSmoothness = math.clamp(Value / 999, 0.01, 1)
   end,
})

TabCombat:CreateDropdown({
   Name = "🎯 Vị Trí Ngắm",
   Options = {"Head", "HumanoidRootPart"},
   CurrentOption = "Head",
   Callback = function(Option)
      Settings.AimTarget = Option[1]
   end,
})

local function IsVisible(targetPart)
    local raycastParams = RaycastParams.new()
    raycastParams.FilterType = Enum.RaycastFilterType.Exclude
    raycastParams.FilterDescendantsInstances = {LocalPlayer.Character, Camera}
    
    local ray = workspace:Raycast(Camera.CFrame.Position, targetPart.Position - Camera.CFrame.Position, raycastParams)
    if ray and ray.Instance then
        return ray.Instance:IsDescendantOf(targetPart.Parent)
    end
    return false
end

RunService.RenderStepped:Connect(function()
    -- Cố định FOV tâm chính giữa màn hình
    local centerPos = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
    FOVCircle.Position = centerPos

    if Settings.Aimbot then
        local NearestPlayer = nil
        local ShortestDistance = FOVCircle.Radius
        
        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild(Settings.AimTarget) then
                local humanoid = plr.Character:FindFirstChildOfClass("Humanoid")
                
                -- Bỏ qua nếu người chơi đó đã chết (Health <= 0)
                if humanoid and humanoid.Health > 0 then
                    local part = plr.Character[Settings.AimTarget]
                    local pos, onScreen = Camera:WorldToViewportPoint(part.Position)
                    local magnitude = (Vector2.new(pos.X, pos.Y) - centerPos).Magnitude
                    
                    if onScreen and magnitude < ShortestDistance then
                        if IsVisible(part) then
                            ShortestDistance = magnitude
                            NearestPlayer = plr
                        end
                    end
                end
            end
        end
        
        if NearestPlayer then
            local targetCFrame = CFrame.new(Camera.CFrame.Position, NearestPlayer.Character[Settings.AimTarget].Position)
            Camera.CFrame = Camera.CFrame:Lerp(targetCFrame, Settings.AimSmoothness)
        end
    end
end)

-- 4. TAB ESP (Persistent - Không mất khi chết)
local function ApplyPlayerESP(plr)
    if plr == LocalPlayer then return end
    
    local function AddHighlight(character)
        if not character then return end
        local highlight = character:WaitForChild("ALB_ESP", 1) or Instance.new("Highlight")
        highlight.Name = "ALB_ESP"
        highlight.FillColor = Color3.fromRGB(140, 0, 255)
        highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
        highlight.Parent = character
        highlight.Enabled = Settings.PlayerESP
    end

    if plr.Character then AddHighlight(plr.Character) end
    plr.CharacterAdded:Connect(function(newChar)
        task.wait(0.5)
        if Settings.PlayerESP then
            AddHighlight(newChar)
        end
    end)
end

for _, p in pairs(Players:GetPlayers()) do ApplyPlayerESP(p) end
Players.PlayerAdded:Connect(ApplyPlayerESP)

TabESP:CreateToggle({
   Name = "👤 Player ESP (Bền Vững - Định Vị Tím)",
   CurrentValue = false,
   Callback = function(Value)
      Settings.PlayerESP = Value
      for _, plr in pairs(Players:GetPlayers()) do
          if plr ~= LocalPlayer and plr.Character then
              local highlight = plr.Character:FindFirstChild("ALB_ESP")
              if Value then
                  if not highlight then
                      highlight = Instance.new("Highlight")
                      highlight.Name = "ALB_ESP"
                      highlight.FillColor = Color3.fromRGB(140, 0, 255)
                      highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                      highlight.Parent = plr.Character
                  end
                  highlight.Enabled = true
              else
                  if highlight then highlight:Destroy() end
              end
          end
      end
   end,
})

-- Item ESP với Bảng Tên Tím + Khoảng Cách
TabESP:CreateToggle({
   Name = "💎 Item ESP (Định Vị Vật Phẩm Tím)",
   CurrentValue = false,
   Callback = function(Value)
      Settings.ItemESP = Value
      task.spawn(function()
          while Settings.ItemESP do
              for _, v in pairs(workspace:GetDescendants()) do
                  if v:IsA("Tool") or v:IsA("TouchTransmitter") then
                      local itemPart = v:IsA("Tool") and (v:FindFirstChild("Handle") or v:FindFirstChildOfClass("Part")) or v.Parent
                      if itemPart and itemPart:IsA("BasePart") and not itemPart:FindFirstChild("ItemESP_Gui") then
                          local billboard = Instance.new("BillboardGui")
                          billboard.Name = "ItemESP_Gui"
                          billboard.AlwaysOnTop = true
                          billboard.Size = UDim2.new(0, 100, 0, 30)
                          billboard.Parent = itemPart

                          local textLabel = Instance.new("TextLabel")
                          textLabel.Size = UDim2.new(1, 0, 1, 0)
                          textLabel.BackgroundTransparency = 1
                          textLabel.TextColor3 = Color3.fromRGB(180, 0, 255)
                          textLabel.TextStrokeTransparency = 0
                          textLabel.Font = Enum.Font.SourceSansBold
                          textLabel.TextSize = 13
                          textLabel.Parent = billboard

                          task.spawn(function()
                              while Settings.ItemESP and itemPart and itemPart.Parent do
                                  if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                                      local dist = math.floor((itemPart.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude)
                                      textLabel.Text = "💎 " .. v.Name .. " [" .. dist .. "m]"
                                  end
                                  task.wait(0.2)
                              end
                              billboard:Destroy()
                          end)
                      end
                  end
              end
              task.wait(1)
          end
      end)
   end,
})

-- 5. TAB VISUALS & UTILITIES
TabVisuals:CreateToggle({
   Name = "🔍 X-Ray (Nhìn Xuyên Tường)",
   CurrentValue = false,
   Callback = function(Value)
      for _, obj in pairs(workspace:GetDescendants()) do
          if obj:IsA("BasePart") and not obj.Parent:FindFirstChild("Humanoid") then
              obj.LocalTransparencyModifier = Value and 0.6 or 0
          end
      end
   end,
})

TabVisuals:CreateButton({
   Name = "🧹 FPS Boost 1 - Xóa Cỏ & Rác",
   Callback = function()
      for _, v in pairs(workspace:GetDescendants()) do
          if v:IsA("BasePart") and (v.Name:lower():find("grass") or v.Name:lower():find("rock")) then
              v:Destroy()
          end
      end
      sethiddenproperty(workspace.Terrain, "Decoration", false)
   end,
})

TabVisuals:CreateButton({
   Name = "🧱 FPS Boost 2 - Đồ Họa Minecraft",
   Callback = function()
      for _, v in pairs(workspace:GetDescendants()) do
          if v:IsA("BasePart") then
              v.Material = Enum.Material.SmoothPlastic
          end
      end
   end,
})

TabVisuals:CreateButton({
   Name = "🌪️ FPS Boost 3 - Đồ Họa Xám Nhạt Tối Giản",
   Callback = function()
      local lighting = game:GetService("Lighting")
      lighting.GlobalShadows = false
      lighting.FogEnd = 9e9
      
      for _, v in pairs(workspace:GetDescendants()) do
          if v:IsA("BasePart") then
              v.Material = Enum.Material.SmoothPlastic
              v.Color = Color3.fromRGB(180, 180, 180)
              v.CastShadow = false
          elseif v:IsA("Decal") or v:IsA("Texture") then
              v:Destroy()
          end
      end
      
      if workspace:FindFirstChildOfClass("Terrain") then
          local terrain = workspace:FindFirstChildOfClass("Terrain")
          terrain.WaterWaveSize = 0
          terrain.WaterWaveSpeed = 0
          terrain.WaterReflectance = 0
          terrain.WaterTransparency = 0
          sethiddenproperty(terrain, "Decoration", false)
      end
   end,
})

TabVisuals:CreateButton({
   Name = "🔄 Chuyển Server Ít Người (Server Hop)",
   Callback = function()
      local TeleportService = game:GetService("TeleportService")
      local HttpService = game:GetService("HttpService")
      local Servers = HttpService:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/"..game.PlaceId.."/servers/Public?sortOrder=Asc&limit=100"))
      for _, s in pairs(Servers.data) do
          if s.playing < s.maxPlayers and s.id ~= game.JobId then
              TeleportService:TeleportToPlaceInstance(game.PlaceId, s.id)
              break
          end
      end
   end,
})

-- 6. TAB SCRIPTS KHÁC
TabScripts:CreateButton({
   Name = "📜 Tải Homelander Script",
   Callback = function()
      loadstring(game:HttpGet("https://raw.githubusercontent.com/giobolqv1/homelander-by-Giobolqv1-/refs/heads/main/homelander.lua"))()
   end,
})

-- Auto Re-Execute On Teleport
local QueueOnTeleport = (syn and syn.queue_on_teleport) or queue_on_teleport or (fluxus and fluxus.queue_on_teleport)
if QueueOnTeleport then
    QueueOnTeleport([[
        loadstring(game:HttpGet("https://raw.githubusercontent.com/giobolqv1/homelander-by-Giobolqv1-/refs/heads/main/homelander.lua"))()
    ]])
end
