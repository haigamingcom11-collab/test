# test
-- ALB HUB - Steal An Egg Script (Mobile & Delta X Optimized)
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- Fix reset khi chết (ResetOnSpawn = false)
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "ALBHub_StealAnEgg"
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

if gethui then
    ScreenGui.Parent = gethui()
elseif syn and syn.protect_gui then
    syn.protect_gui(ScreenGui)
    ScreenGui.Parent = game:GetService("CoreGui")
else
    ScreenGui.Parent = game:GetService("CoreGui")
end

-- Khung Main Menu
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 340, 0, 240)
MainFrame.Position = UDim2.new(0.5, -170, 0.5, -120)
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 12, 22)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Parent = ScreenGui

local MainUICorner = Instance.new("UICorner", MainFrame)
MainUICorner.CornerRadius = UDim.new(0, 12)

-- Đèn LED Viền (UIStroke)
local UIStroke = Instance.new("UIStroke")
UIStroke.Parent = MainFrame
UIStroke.Thickness = 2.5
UIStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
UIStroke.Color = Color3.fromRGB(180, 0, 255)

-- Hiệu ứng LED nhấp nháy êm (Pulse Tween)
local ledTweenInfo = TweenInfo.new(1.5, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true)
local ledTween = TweenService:Create(UIStroke, ledTweenInfo, {Color = Color3.fromRGB(90, 0, 150)})
ledTween:Play()

-- Header Bar
local Header = Instance.new("Frame", MainFrame)
Header.Size = UDim2.new(1, 0, 0, 35)
Header.BackgroundColor3 = Color3.fromRGB(25, 18, 38)
Header.BorderSizePixel = 0

local HeaderCorner = Instance.new("UICorner", Header)
HeaderCorner.CornerRadius = UDim.new(0, 12)

local Title = Instance.new("TextLabel", Header)
Title.Size = UDim2.new(1, -100, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.Text = "⚡ ALB HUB | Steal An Egg"
Title.TextColor3 = Color3.fromRGB(220, 150, 255)
Title.TextSize = 14
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.BackgroundTransparency = 1

-- Nút Thu gọn (-)
local MinimizeBtn = Instance.new("TextButton", Header)
MinimizeBtn.Size = UDim2.new(0, 25, 0, 25)
MinimizeBtn.Position = UDim2.new(1, -60, 0, 5)
MinimizeBtn.Text = "-"
MinimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeBtn.BackgroundColor3 = Color3.fromRGB(40, 30, 60)
MinimizeBtn.Font = Enum.Font.GothamBold
Instance.new("UICorner", MinimizeBtn).CornerRadius = UDim.new(0, 6)

-- Nút Thu vào góc (X)
local CloseBtn = Instance.new("TextButton", Header)
CloseBtn.Size = UDim2.new(0, 25, 0, 25)
CloseBtn.Position = UDim2.new(1, -30, 0, 5)
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Color3.fromRGB(255, 80, 80)
CloseBtn.BackgroundColor3 = Color3.fromRGB(40, 30, 60)
CloseBtn.Font = Enum.Font.GothamBold
Instance.new("UICorner", CloseBtn).CornerRadius = UDim.new(0, 6)

-- Nút Tròn Mở Tắt Nhanh (Floating Icon)
local OpenIcon = Instance.new("TextButton", ScreenGui)
OpenIcon.Size = UDim2.new(0, 45, 0, 45)
OpenIcon.Position = UDim2.new(0.05, 0, 0.2, 0)
OpenIcon.Text = "ALB"
OpenIcon.TextColor3 = Color3.fromRGB(255, 255, 255)
OpenIcon.BackgroundColor3 = Color3.fromRGB(20, 15, 30)
OpenIcon.Font = Enum.Font.GothamBold
OpenIcon.Visible = false
Instance.new("UICorner", OpenIcon).CornerRadius = UDim.new(1, 0)

local IconStroke = Instance.new("UIStroke", OpenIcon)
IconStroke.Thickness = 2
IconStroke.Color = Color3.fromRGB(180, 0, 255)

-- Tab Selection
local TabHolder = Instance.new("Frame", MainFrame)
TabHolder.Size = UDim2.new(1, -20, 0, 30)
TabHolder.Position = UDim2.new(0, 10, 0, 40)
TabHolder.BackgroundTransparency = 1

local Tab1Btn = Instance.new("TextButton", TabHolder)
Tab1Btn.Size = UDim2.new(0.48, 0, 1, 0)
Tab1Btn.Text = "Main Steal"
Tab1Btn.TextColor3 = Color3.fromRGB(255, 255, 255)
Tab1Btn.BackgroundColor3 = Color3.fromRGB(140, 0, 230)
Tab1Btn.Font = Enum.Font.GothamBold
Instance.new("UICorner", Tab1Btn).CornerRadius = UDim.new(0, 6)

local Tab2Btn = Instance.new("TextButton", TabHolder)
Tab2Btn.Size = UDim2.new(0.48, 0, 1, 0)
Tab2Btn.Position = UDim2.new(0.52, 0, 0, 0)
Tab2Btn.Text = "Player & Misc"
Tab2Btn.TextColor3 = Color3.fromRGB(200, 200, 200)
Tab2Btn.BackgroundColor3 = Color3.fromRGB(35, 25, 50)
Tab2Btn.Font = Enum.Font.GothamBold
Instance.new("UICorner", Tab2Btn).CornerRadius = UDim.new(0, 6)

-- Pages
local Page1 = Instance.new("ScrollingFrame", MainFrame)
Page1.Size = UDim2.new(1, -20, 1, -80)
Page1.Position = UDim2.new(0, 10, 0, 75)
Page1.BackgroundTransparency = 1
Page1.ScrollBarThickness = 2
Page1.CanvasSize = UDim2.new(0, 0, 0, 180)

local Page2 = Instance.new("ScrollingFrame", MainFrame)
Page2.Size = UDim2.new(1, -20, 1, -80)
Page2.Position = UDim2.new(0, 10, 0, 75)
Page2.BackgroundTransparency = 1
Page2.ScrollBarThickness = 2
Page2.Visible = false
Page2.CanvasSize = UDim2.new(0, 0, 0, 180)

local UIList1 = Instance.new("UIListLayout", Page1)
UIList1.Padding = UDim.new(0, 6)
local UIList2 = Instance.new("UIListLayout", Page2)
UIList2.Padding = UDim.new(0, 6)

-- Hệ thống Kéo Thả (Drag GUI - Hoạt động chuẩn cho Delta X Mobile)
local dragging, dragInput, dragStart, startPos
local function update(input)
    local delta = input.Position - dragStart
    MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

Header.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = MainFrame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then dragging = false end
        end)
    end
end)

Header.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if input == dragInput and dragging then update(input) end
end)

-- Tạo Toggle Button Hàm Tùy Biến
local function CreateToggle(parent, text, callback)
    local btn = Instance.new("TextButton", parent)
    btn.Size = UDim2.new(1, 0, 0, 35)
    btn.BackgroundColor3 = Color3.fromRGB(28, 20, 42)
    btn.Text = "  " .. text
    btn.TextColor3 = Color3.fromRGB(200, 200, 200)
    btn.Font = Enum.Font.Gotham
    btn.TextXAlignment = Enum.TextXAlignment.Left
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)

    local status = Instance.new("Frame", btn)
    status.Size = UDim2.new(0, 16, 0, 16)
    status.Position = UDim2.new(1, -26, 0.5, -8)
    status.BackgroundColor3 = Color3.fromRGB(60, 50, 80)
    Instance.new("UICorner", status).CornerRadius = UDim.new(0, 4)

    local enabled = false
    btn.MouseButton1Click:Connect(function()
        enabled = not enabled
        if enabled then
            status.BackgroundColor3 = Color3.fromRGB(180, 0, 255)
            btn.TextColor3 = Color3.fromRGB(255, 255, 255)
        else
            status.BackgroundColor3 = Color3.fromRGB(60, 50, 80)
            btn.TextColor3 = Color3.fromRGB(200, 200, 200)
        end
        pcall(callback, enabled)
    end)
end

-- --- TÍNH NĂNG TAB 1: MAIN STEAL (OPTIMIZED) ---
CreateToggle(Page1, "Auto Steal Egg (Tự Trộm Trứng)", function(v)
    _G.AutoSteal = v
    if v then
        task.spawn(function()
            while _G.AutoSteal do
                task.wait(0.2) -- Tăng delay nhẹ để giảm tải CPU Mobile
                pcall(function()
                    for _, obj in ipairs(workspace:GetChildren()) do
                        if obj:IsA("ProximityPrompt") and (obj.ObjectText:lower():find("egg") or obj.ActionText:lower():find("steal")) then
                            fireproximityprompt(obj)
                        else
                            for _, child in ipairs(obj:GetChildren()) do
                                if child:IsA("ProximityPrompt") and (child.ObjectText:lower():find("egg") or child.ActionText:lower():find("steal")) then
                                    fireproximityprompt(child)
                                end
                            end
                        end
                    end
                end)
            end
        end)
    end
end)

CreateToggle(Page1, "Auto Bring Eggs (Gom Trứng)", function(v)
    _G.AutoBring = v
    if v then
        task.spawn(function()
            while _G.AutoBring do
                task.wait(0.3) -- Tối ưu thời gian chờ tránh spam CFrame gây lag
                pcall(function()
                    local char = LocalPlayer.Character
                    local hrp = char and char:FindFirstChild("HumanoidRootPart")
                    if hrp then
                        for _, item in ipairs(workspace:GetChildren()) do
                            if item:IsA("BasePart") and item.Name:lower():find("egg") then
                                item.CFrame = hrp.CFrame
                            end
                        end
                    end
                end)
            end
        end)
    end
end)

-- --- TÍNH NĂNG TAB 2: PLAYER & MISC (OPTIMIZED) ---
local speedConnection
CreateToggle(Page2, "Tăng Tốc Độ (Speed Boost)", function(v)
    _G.Speed = v
    local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    local hum = char:WaitForChild("Humanoid")
    
    if speedConnection then speedConnection:Disconnect() end
    
    if _G.Speed then
        hum.WalkSpeed = 32
        -- Quản lý thay đổi WalkSpeed thông minh hơn (Không dùng RenderStepped)
        speedConnection = hum:GetPropertyChangedSignal("WalkSpeed"):Connect(function()
            if _G.Speed and hum.WalkSpeed ~= 32 then
                hum.WalkSpeed = 32
            end
        end)
    else
        hum.WalkSpeed = 16
    end
end)

CreateToggle(Page2, "Nhảy Cao (Infinite Jump)", function(v)
    _G.InfJump = v
end)

UserInputService.JumpRequest:Connect(function()
    if _G.InfJump and LocalPlayer.Character then
        local hum = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if hum then
            hum:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)

-- Xử lý chuyển Tab
Tab1Btn.MouseButton1Click:Connect(function()
    Page1.Visible = true
    Page2.Visible = false
    Tab1Btn.BackgroundColor3 = Color3.fromRGB(140, 0, 230)
    Tab2Btn.BackgroundColor3 = Color3.fromRGB(35, 25, 50)
end)

Tab2Btn.MouseButton1Click:Connect(function()
    Page1.Visible = false
    Page2.Visible = true
    Tab2Btn.BackgroundColor3 = Color3.fromRGB(140, 0, 230)
    Tab1Btn.BackgroundColor3 = Color3.fromRGB(35, 25, 50)
end)

-- Xử lý Nút Thu Gọn (-) và Tắt Góc (X)
local isMinimized = false
MinimizeBtn.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    Page1.Visible = not isMinimized and Tab1Btn.BackgroundColor3 == Color3.fromRGB(140, 0, 230)
    Page2.Visible = not isMinimized and Tab2Btn.BackgroundColor3 == Color3.fromRGB(140, 0, 230)
    TabHolder.Visible = not isMinimized
    MainFrame.Size = isMinimized and UDim2.new(0, 340, 0, 35) or UDim2.new(0, 340, 0, 240)
end)

CloseBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    OpenIcon.Visible = true
end)

OpenIcon.MouseButton1Click:Connect(function()
    MainFrame.Visible = true
    OpenIcon.Visible = false
end)
])
end
