Local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Stats = game:GetService("Stats")
local Lighting = game:GetService("Lighting")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local HttpService = game:GetService("HttpService")

local LocalPlayer = Players.LocalPlayer
local IS_OWNER = (LocalPlayer.Name == "LifzxAfter2")

-- Hapus GUI lama jika ada
if CoreGui:FindFirstChild("StealAnEggUI") then
    CoreGui.StealAnEggUI:Destroy()
end

-- ScreenGui Utama
local StealAnEggUI = Instance.new("ScreenGui")
StealAnEggUI.Name = "StealAnEggUI"
StealAnEggUI.Parent = CoreGui
StealAnEggUI.ResetOnSpawn = false
StealAnEggUI.IgnoreGuiInset = true

-- Frame Utama
local MainFrame = Instance.new("ImageLabel")
MainFrame.Name = "MainFrame"
MainFrame.Parent = StealAnEggUI
MainFrame.Size = UDim2.new(0, 500, 0, 350)
MainFrame.Position = UDim2.new(0.5, -250, 0.5, -175)
MainFrame.Image = "rbxassetid://137867255261184"
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.ClipsDescendants = true

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 10)
MainCorner.Parent = MainFrame

-- Profil Avatar Bulat
local AvatarImage = Instance.new("ImageLabel")
AvatarImage.Name = "AvatarImage"
AvatarImage.Parent = MainFrame
AvatarImage.Size = UDim2.new(0, 80, 0, 80)
AvatarImage.Position = UDim2.new(0, 15, 0, 10)
AvatarImage.BackgroundTransparency = 1

local AvatarCorner = Instance.new("UICorner")
AvatarCorner.CornerRadius = UDim.new(1, 0)
AvatarCorner.Parent = AvatarImage

task.spawn(function()
    local userId = LocalPlayer.UserId
    local thumbType = Enum.ThumbnailType.HeadShot
    local thumbSize = Enum.ThumbnailSize.Size420x420
    local content, isReady = Players:GetUserThumbnailAsync(userId, thumbType, thumbSize)
    if isReady then
        AvatarImage.Image = content
    end
end)

-- Tombol Atas
local TopButtons = Instance.new("Frame")
TopButtons.Parent = MainFrame
TopButtons.BackgroundTransparency = 1
TopButtons.Position = UDim2.new(1, -70, 0, 5)
TopButtons.Size = UDim2.new(0, 65, 0, 25)

local MinBtn = Instance.new("TextButton")
MinBtn.Parent = TopButtons
MinBtn.Size = UDim2.new(0, 25, 0, 25)
MinBtn.Position = UDim2.new(0, 0, 0, 0)
MinBtn.BackgroundTransparency = 1
MinBtn.Text = "─"
MinBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinBtn.TextSize = 18
MinBtn.Font = Enum.Font.SourceSansBold

local CloseBtn = Instance.new("TextButton")
CloseBtn.Parent = TopButtons
CloseBtn.Size = UDim2.new(0, 25, 0, 25)
CloseBtn.Position = UDim2.new(0, 35, 0, 0)
CloseBtn.BackgroundTransparency = 1
CloseBtn.Text = "✕"
CloseBtn.TextColor3 = Color3.fromRGB(255, 0, 0)
CloseBtn.TextSize = 20
CloseBtn.Font = Enum.Font.SourceSansBold

-- Sidebar Kiri
local Sidebar = Instance.new("Frame")
Sidebar.Parent = MainFrame
Sidebar.BackgroundTransparency = 1
Sidebar.Position = UDim2.new(0, 10, 0, 100)
Sidebar.Size = UDim2.new(0, 100, 0, 240)

local UIListSidebar = Instance.new("UIListLayout")
UIListSidebar.Parent = Sidebar
UIListSidebar.SortOrder = Enum.SortOrder.LayoutOrder
UIListSidebar.Padding = UDim.new(0, 4)

-- Containers
local RightContainer = Instance.new("Frame")
RightContainer.Name = "RightContainer"
RightContainer.Parent = MainFrame
RightContainer.BackgroundTransparency = 1
RightContainer.Position = UDim2.new(0, 120, 0, 10)
RightContainer.Size = UDim2.new(0, 370, 0, 330)

local PlayerTabContainer = Instance.new("Frame")
PlayerTabContainer.Name = "PlayerTabContainer"
PlayerTabContainer.Parent = MainFrame
PlayerTabContainer.BackgroundTransparency = 1
PlayerTabContainer.Position = UDim2.new(0, 140, 0, 10)
PlayerTabContainer.Size = UDim2.new(0, 350, 0, 330)
PlayerTabContainer.Visible = false

local TampilanTabContainer = Instance.new("Frame")
TampilanTabContainer.Name = "TampilanTabContainer"
TampilanTabContainer.Parent = MainFrame
TampilanTabContainer.BackgroundTransparency = 1
TampilanTabContainer.Position = UDim2.new(0, 125, 0, 80)
TampilanTabContainer.Size = UDim2.new(0, 365, 0, 260)
TampilanTabContainer.Visible = false

-- ADMIN CONTAINER
local AdminTabContainer = Instance.new("Frame")
AdminTabContainer.Name = "AdminTabContainer"
AdminTabContainer.Parent = MainFrame
AdminTabContainer.BackgroundTransparency = 1
AdminTabContainer.Position = UDim2.new(0, 120, 0, 10)
AdminTabContainer.Size = UDim2.new(0, 370, 0, 330)
AdminTabContainer.Visible = false

local BottomBar

-- Management Sistem Switching Tab
local tabs = {"Main", "Player", "Tampilan", "ADMIN"}

for _, tabName in ipairs(tabs) do
    local btn = Instance.new("TextButton")
    btn.Parent = Sidebar
    btn.Size = UDim2.new(1, 0, 0, 28)
    btn.BackgroundColor3 = Color3.fromRGB(240, 240, 240)
    btn.Text = tabName
    btn.TextColor3 = Color3.fromRGB(0, 0, 0)
    btn.Font = Enum.Font.SourceSansBold
    btn.TextSize = 14

    local corner = Instance.new("UICorner")     
    corner.CornerRadius = UDim.new(0, 8)     
    corner.Parent = btn     

    local stroke = Instance.new("UIStroke")     
    stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border     
    stroke.Color = Color3.fromRGB(0, 0, 0)     
    stroke.Thickness = 1.5     
    stroke.Parent = btn    

    btn.MouseButton1Click:Connect(function()  
        RightContainer.Visible = (tabName == "Main")
        PlayerTabContainer.Visible = (tabName == "Player")
        TampilanTabContainer.Visible = (tabName == "Tampilan")
        AdminTabContainer.Visible = (tabName == "ADMIN")
        
        if BottomBar then 
            BottomBar.Visible = (tabName == "Player") 
        end  
    end)
end

local OpenScriptBtn = Instance.new("TextButton")
OpenScriptBtn.Parent = RightContainer
OpenScriptBtn.Size = UDim2.new(0, 180, 0, 30)
OpenScriptBtn.Position = UDim2.new(0.5, -90, 0, 0)
OpenScriptBtn.BackgroundColor3 = Color3.fromRGB(245, 245, 245)
OpenScriptBtn.Text = "OPEN SCRIPT"
OpenScriptBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
OpenScriptBtn.Font = Enum.Font.SourceSansBold
OpenScriptBtn.TextSize = 16

local OpenCorner = Instance.new("UICorner")
OpenCorner.CornerRadius = UDim.new(0, 8)
OpenCorner.Parent = OpenScriptBtn

local OpenStroke = Instance.new("UIStroke")
OpenStroke.Color = Color3.fromRGB(255, 0, 0)
OpenStroke.Thickness = 1.5
OpenStroke.Parent = OpenScriptBtn

local ScriptScroll = Instance.new("ScrollingFrame")
ScriptScroll.Parent = RightContainer
ScriptScroll.BackgroundTransparency = 1
ScriptScroll.Position = UDim2.new(0, 0, 0, 35)
ScriptScroll.Size = UDim2.new(1, 0, 1, -35)
ScriptScroll.CanvasSize = UDim2.new(0, 0, 0, 750)
ScriptScroll.ScrollBarThickness = 4

local ScrollList = Instance.new("UIListLayout")
ScrollList.Parent = ScriptScroll
ScrollList.SortOrder = Enum.SortOrder.LayoutOrder
ScrollList.Padding = UDim.new(0, 6)

local scriptsData = {
    {Header = "🔐 NO KEY"},
    {Name = "LENON HUB", Load = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/lennonxscripts/lennonhubv4/refs/heads/main/stealanegg",true))()]]},
    {Name = "CHILI HUB", Load = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/tienkhanh1/spicy/main/Chilli.lua"))()]]},
    {Name = "SENA BETA", Load = [[loadstring(game:HttpGet("https://senahub.xyz/raw/loader"))()]]},
    {Name = "PULSE HUB", Load = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/PulseZax/Loader/refs/heads/main/.lua"))()]]},
    {Name = "ALL SC FAKE ADMIN", Load = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/Dayvinksthik/Script/refs/heads/main/Games/JoshBNS-Crack.lua"))()]]},
    {Name = "DHII's HUB", Load = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/dhiisepertiorang/dhii_kick.lua/refs/heads/main/dhiitoaa.lua"))()]]},
    {Name = "WHEZEUS HUB", Load = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/Wzeus-NTH/Wzeusno1/main/Wzeus/nthzz"))()]]},
    {Name = "NEVA HUB", Load = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/VEZ2/NEVAHUB/main/2"))()]]},
    {Name = "MIRANDA", Load = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/miirandahub/loader/refs/heads/main/stealeggies"))()]]},
    {Header = " KEYLES / KEY"},
    {Name = "SENA V6", Load = [[loadstring(game:HttpGet("https://senahub.xyz/senav6"))()]]},
    {Name = "SYS HUB", Load = [[loadstring(game:HttpGet("https://syshub.fun/free"))()]]},
    {Name = "BIGFROOT", Load = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/hanniii1/Loader/refs/heads/main/BFLoader.lua"))()]]},
    {Name = "LIMBO HUB", Load = [[loadstring(game:HttpGet("https://limbohub.my.id/loader.lua"))()]]},
    {Name = "UNKNOWN HUB", Load = [[loadstring(game:HttpGet("https://unknownhub.win/api/projects/54474b4c5d5a4f459909c4cb70e7b4f3/loader"))()]]},
    {Name = "AJJANS HUB", Load = [[loadstring(game:HttpGet("https://api.luarmor.net/files/v4/loaders/359e97f8618e9008afe5f496184ebb7c.lua"))()]]},
    {Name = "KING VYPER", Load = [[loadstring(game:HttpGet('https://kingvypers.site/raw/TrialLoader'))()]]},
    {Name = "FYY COMMUNITY", Load = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/napun87/stealanegg/refs/heads/main/fly.lua"))()]]}
}

local selectedScript = nil
local allCheckButtons = {}

for _, item in ipairs(scriptsData) do
    if item.Header then
        local headerLabel = Instance.new("TextLabel")
        headerLabel.Parent = ScriptScroll
        headerLabel.Size = UDim2.new(1, -10, 0, 25)
        headerLabel.BackgroundTransparency = 1
        headerLabel.Text = item.Header
        headerLabel.TextColor3 = Color3.fromRGB(255, 50, 50)
        headerLabel.Font = Enum.Font.SourceSansBold
        headerLabel.TextSize = 18
    else
        local scriptFrame = Instance.new("Frame")
        scriptFrame.Parent = ScriptScroll
        scriptFrame.Size = UDim2.new(1, -10, 0, 32)
        scriptFrame.BackgroundColor3 = Color3.fromRGB(245, 245, 245)

        local frameCorner = Instance.new("UICorner")     
        frameCorner.CornerRadius = UDim.new(0, 8)     
        frameCorner.Parent = scriptFrame     

        local frameStroke = Instance.new("UIStroke")     
        frameStroke.Color = Color3.fromRGB(0, 0, 0)     
        frameStroke.Thickness = 1.5     
        frameStroke.Parent = scriptFrame     

        local titleLabel = Instance.new("TextLabel")     
        titleLabel.Parent = scriptFrame     
        titleLabel.Size = UDim2.new(1, -50, 1, 0)     
        titleLabel.Position = UDim2.new(0, 10, 0, 0)     
        titleLabel.BackgroundTransparency = 1     
        titleLabel.Text = item.Name     
        titleLabel.TextColor3 = Color3.fromRGB(0, 0, 0)     
        titleLabel.Font = Enum.Font.SourceSansBold     
        titleLabel.TextSize = 16     
        titleLabel.TextXAlignment = Enum.TextXAlignment.Center     

        local checkBtn = Instance.new("TextButton")     
        checkBtn.Parent = scriptFrame     
        checkBtn.Size = UDim2.new(0, 22, 0, 22)     
        checkBtn.Position = UDim2.new(1, -30, 0.5, -11)     
        checkBtn.BackgroundColor3 = Color3.fromRGB(100, 100, 100)     
        checkBtn.Text = ""     
        checkBtn.TextColor3 = Color3.fromRGB(0, 0, 0)     
        checkBtn.Font = Enum.Font.SourceSansBold     
        checkBtn.TextSize = 16     

        local checkCorner = Instance.new("UICorner")     
        checkCorner.CornerRadius = UDim.new(0, 4)     
        checkCorner.Parent = checkBtn     

        table.insert(allCheckButtons, {Button = checkBtn, Code = item.Load})     

        checkBtn.MouseButton1Click:Connect(function()     
            for _, v in ipairs(allCheckButtons) do     
                v.Button.Text = ""     
                v.Button.BackgroundColor3 = Color3.fromRGB(100, 100, 100)     
            end     
            checkBtn.Text = "✓"     
            checkBtn.BackgroundColor3 = Color3.fromRGB(200, 200, 200)     
            selectedScript = item.Load     
        end)     
    end
end

OpenScriptBtn.MouseButton1Click:Connect(function()
    if selectedScript then
        local func, err = loadstring(selectedScript)
        if func then
            task.spawn(func)
        else
            warn("Gagal mengeksekusi script: " .. tostring(err))
        end
    end
end)

-- Box Info Akun & Jaringan
local InfoBox = Instance.new("Frame")
InfoBox.Parent = PlayerTabContainer
InfoBox.Size = UDim2.new(0, 175, 0, 80)
InfoBox.Position = UDim2.new(0, 0, 0, 0)
InfoBox.BackgroundColor3 = Color3.fromRGB(240, 240, 240)

local InfoCorner = Instance.new("UICorner")
InfoCorner.CornerRadius = UDim.new(0, 12)
InfoCorner.Parent = InfoBox

local InfoStroke = Instance.new("UIStroke")
InfoStroke.Color = Color3.fromRGB(0, 0, 0)
InfoStroke.Thickness = 1.5
InfoStroke.Parent = InfoBox

local InfoText = Instance.new("TextLabel")
InfoText.Parent = InfoBox
InfoText.Size = UDim2.new(1, -10, 1, -10)
InfoText.Position = UDim2.new(0, 5, 0, 5)
InfoText.BackgroundTransparency = 1
InfoText.TextColor3 = Color3.fromRGB(0, 0, 0)
InfoText.Font = Enum.Font.SourceSansBold
InfoText.TextSize = 14
InfoText.TextXAlignment = Enum.TextXAlignment.Left
InfoText.TextYAlignment = Enum.TextYAlignment.Top

local accountAgeDays = LocalPlayer.AccountAge
local deviceType = UserInputService.TouchEnabled and "Android" or "PC"

task.spawn(function()
    while task.wait(1) do
        local pingVal = 0
        pcall(function()
            pingVal = math.floor(Stats.Network.ServerStatsItem["Data Ping"]:GetValue())
        end)
        InfoText.Text = string.format("Ping%d+\nUkuran Akun :%dDay\nUsername:%s\n%s", pingVal, accountAgeDays, LocalPlayer.Name, deviceType)
    end
end)

-- Fungsi Pembuat Custom Toggle Switch
local function createToggle(name, yPos)
    local frame = Instance.new("Frame")
    frame.Parent = PlayerTabContainer
    frame.Size = UDim2.new(0, 185, 0, 36)
    frame.Position = UDim2.new(0, 0, 0, yPos)
    frame.BackgroundColor3 = Color3.fromRGB(240, 240, 240)

    local corner = Instance.new("UICorner")  
    corner.CornerRadius = UDim.new(0, 10)  
    corner.Parent = frame  

    local stroke = Instance.new("UIStroke")  
    stroke.Color = Color3.fromRGB(0, 0, 0)  
    stroke.Thickness = 1.5  
    stroke.Parent = frame  

    local label = Instance.new("TextLabel")  
    label.Parent = frame  
    label.Size = UDim2.new(0, 120, 1, 0)  
    label.Position = UDim2.new(0, 8, 0, 0)  
    label.BackgroundTransparency = 1  
    label.Text = name  
    label.TextColor3 = Color3.fromRGB(0, 0, 0)  
    label.Font = Enum.Font.SourceSansBold  
    label.TextSize = 15  
    label.TextXAlignment = Enum.TextXAlignment.Left  

    local switchBackground = Instance.new("Frame")  
    switchBackground.Parent = frame  
    switchBackground.Size = UDim2.new(0, 48, 0, 24)  
    switchBackground.Position = UDim2.new(1, -54, 0.5, -12)  
    switchBackground.BackgroundColor3 = Color3.fromRGB(0, 0, 0)  

    local switchCorner = Instance.new("UICorner")  
    switchCorner.CornerRadius = UDim.new(1, 0)  
    switchCorner.Parent = switchBackground  

    local circleNode = Instance.new("TextButton")  
    circleNode.Parent = switchBackground  
    circleNode.Size = UDim2.new(0, 20, 0, 20)  
    circleNode.Position = UDim2.new(0, 2, 0.5, -10)  
    circleNode.BackgroundColor3 = Color3.fromRGB(255, 0, 0)  
    circleNode.Text = ""  

    local circleCorner = Instance.new("UICorner")  
    circleCorner.CornerRadius = UDim.new(1, 0)  
    circleCorner.Parent = circleNode  

    local isToggled = false  

    local function setToggleState(state)  
        isToggled = state  
        if isToggled then  
            TweenService:Create(circleNode, TweenInfo.new(0.2), {  
                Position = UDim2.new(1, -22, 0.5, -10),  
                BackgroundColor3 = Color3.fromRGB(0, 255, 0)  
            }):Play()  
        else  
            TweenService:Create(circleNode, TweenInfo.new(0.2), {  
                Position = UDim2.new(0, 2, 0.5, -10),  
                BackgroundColor3 = Color3.fromRGB(255, 0, 0)  
            }):Play()  
        end  
    end  

    return frame, circleNode, function() return isToggled end, setToggleState
end

-- Membuat Toggle Utama
local _, NoClipBtn, getNoClipState, setNoClip = createToggle("NO CLIP", 92)
local _, EspBtn, getEspState, setEsp = createToggle("Esp Player", 136)
local _, RemovePlayerBtn, getRemoveState, setRemove = createToggle("Remove Player", 180)

-- Fitur NO CLIP
local noClipConnection = nil
NoClipBtn.MouseButton1Click:Connect(function()
    setNoClip(not getNoClipState())
    if getNoClipState() then
        noClipConnection = RunService.Stepped:Connect(function()
            if LocalPlayer.Character then
                for _, part in ipairs(LocalPlayer.Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end
        end)
    else
        if noClipConnection then
            noClipConnection:Disconnect()
            noClipConnection = nil
        end
    end
end)

-- Fitur ESP PLAYER
local espHolders = {}
local function removeAllESP()
    for _, item in pairs(espHolders) do
        if item then item:Destroy() end
    end
    espHolders = {}
end

local function applyESP(plr)
    if plr == LocalPlayer or not plr.Character then return end

    local head = plr.Character:FindFirstChild("Head")   
    if not head then return end   

    local bb = Instance.new("BillboardGui")   
    bb.Name = "PlayerESP"   
    bb.Adornee = head   
    bb.Size = UDim2.new(0, 100, 0, 50)   
    bb.StudsOffset = Vector3.new(0, 2.5, 0)   
    bb.AlwaysOnTop = true   
    bb.Parent = CoreGui   

    local img = Instance.new("ImageLabel")   
    img.Parent = bb   
    img.Size = UDim2.new(0, 24, 0, 24)   
    img.Position = UDim2.new(0.5, -12, 0, 0)   
    img.BackgroundTransparency = 1   

    local imgCorner = Instance.new("UICorner")   
    imgCorner.CornerRadius = UDim.new(1, 0)   
    imgCorner.Parent = img   

    task.spawn(function()   
        local content, isReady = Players:GetUserThumbnailAsync(plr.UserId, Enum.ThumbnailType.HeadShot, Enum.ThumbnailSize.Size420x420)   
        if isReady then img.Image = content end   
    end)   

    local lbl = Instance.new("TextLabel")   
    lbl.Parent = bb   
    lbl.Size = UDim2.new(1, 0, 0, 20)   
    lbl.Position = UDim2.new(0, 0, 0, 25)   
    lbl.BackgroundTransparency = 1   
    lbl.Text = plr.Name   
    lbl.TextColor3 = Color3.fromRGB(255, 0, 0)   
    lbl.Font = Enum.Font.SourceSansBold   
    lbl.TextSize = 14   

    local stroke = Instance.new("UIStroke")   
    stroke.Color = Color3.fromRGB(0, 0, 0)   
    stroke.Thickness = 2   
    stroke.Parent = lbl   

    table.insert(espHolders, bb)
end

EspBtn.MouseButton1Click:Connect(function()
    setEsp(not getEspState())
    if getEspState() then
        for _, plr in ipairs(Players:GetPlayers()) do
            applyESP(plr)
        end
    else
        removeAllESP()
    end
end)

Players.PlayerAdded:Connect(function(plr)
    plr.CharacterAdded:Connect(function()
        if getEspState() then
            task.wait(0.5)
            applyESP(plr)
        end
    end)
end)

-- Fitur REMOVE PLAYER
local hiddenCharacters = {}

RemovePlayerBtn.MouseButton1Click:Connect(function()
    setRemove(not getRemoveState())
    if getRemoveState() then
        for _, plr in ipairs(Players:GetPlayers()) do
            if plr ~= LocalPlayer and plr.Character then
                for _, part in ipairs(plr.Character:GetDescendants()) do
                    if part:IsA("BasePart") or part:IsA("Decal") then
                        if not hiddenCharacters[part] then
                            hiddenCharacters[part] = part.Transparency
                        end
                        part.Transparency = 1
                    end
                end
            end
        end
    else
        for part, oldTrans in pairs(hiddenCharacters) do
            if part and part.Parent then
                part.Transparency = oldTrans
            end
        end
        hiddenCharacters = {}
    end
end)

local function createCheckboxCard(name, pos, callback)
    local frame = Instance.new("Frame")
    frame.Parent = TampilanTabContainer
    frame.Size = UDim2.new(0, 170, 0, 42)
    frame.Position = pos
    frame.BackgroundColor3 = Color3.fromRGB(235, 235, 235)

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 12)
    corner.Parent = frame

    local stroke = Instance.new("UIStroke")
    stroke.Color = Color3.fromRGB(0, 0, 0)
    stroke.Thickness = 1.5
    stroke.Parent = frame

    local label = Instance.new("TextLabel")
    label.Parent = frame
    label.Size = UDim2.new(1, -45, 1, 0)
    label.Position = UDim2.new(0, 10, 0, 0)
    label.BackgroundTransparency = 1
    label.Text = name
    label.TextColor3 = Color3.fromRGB(0, 0, 0)
    label.Font = Enum.Font.SourceSansBold
    label.TextSize = 16
    label.TextXAlignment = Enum.TextXAlignment.Left

    local checkSquare = Instance.new("TextButton")
    checkSquare.Parent = frame
    checkSquare.Size = UDim2.new(0, 26, 0, 26)
    checkSquare.Position = UDim2.new(1, -34, 0.5, -13)
    checkSquare.BackgroundColor3 = Color3.fromRGB(110, 110, 110)
    checkSquare.Text = ""
    checkSquare.TextColor3 = Color3.fromRGB(0, 0, 0)
    checkSquare.Font = Enum.Font.SourceSansBold
    checkSquare.TextSize = 18

    local sqCorner = Instance.new("UICorner")
    sqCorner.CornerRadius = UDim.new(0, 6)
    sqCorner.Parent = checkSquare

    local sqStroke = Instance.new("UIStroke")
    sqStroke.Color = Color3.fromRGB(0, 0, 0)
    sqStroke.Thickness = 1.5
    sqStroke.Parent = checkSquare

    local isChecked = false

    checkSquare.MouseButton1Click:Connect(function()
        isChecked = not isChecked
        if isChecked then
            checkSquare.Text = "✓"
            checkSquare.BackgroundColor3 = Color3.fromRGB(180, 180, 180)
        else
            checkSquare.Text = ""
            checkSquare.BackgroundColor3 = Color3.fromRGB(110, 110, 110)
        end
        callback(isChecked)
    end)
end

-- Variabel & Penyimpanan Fitur Tampilan
local originalLighting = {
    ClockTime = Lighting.ClockTime,
    Ambient = Lighting.Ambient,
    OutdoorAmbient = Lighting.OutdoorAmbient,
    Sky = Lighting:FindFirstChildOfClass("Sky") and Lighting:FindFirstChildOfClass("Sky"):Clone() or nil
}

-- 1. Fps Boats
createCheckboxCard("Fps Boats", UDim2.new(0, 0, 0, 0), function(state)
    if state then
        settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
        for _, v in ipairs(workspace:GetDescendants()) do
            if v:IsA("BasePart") then
                v.Material = Enum.Material.SmoothPlastic
            elseif v:IsA("Decal") or v:IsA("Texture") then
                v.Transparency = 1
            end
        end
    else
        settings().Rendering.QualityLevel = Enum.QualityLevel.Automatic
    end
end)

-- 2. 120Fps
local fps120Conn = nil
createCheckboxCard("120Fps", UDim2.new(0, 185, 0, 0), function(state)
    if state then
        if setfpscap then
            setfpscap(120)
        end
        fps120Conn = RunService.RenderStepped:Connect(function() end)
    else
        if fps120Conn then
            fps120Conn:Disconnect()
            fps120Conn = nil
        end
        if setfpscap then
            setfpscap(60)
        end
    end
end)

-- 3. PC Screen
createCheckboxCard("PC Screen", UDim2.new(0, 0, 0, 52), function(state)
    local camera = workspace.CurrentCamera
    if state then
        camera.FieldOfView = 90
    else
        camera.FieldOfView = 70
    end
end)

-- 4. Anti-night
local antiNightConn = nil
createCheckboxCard("Anti-night", UDim2.new(0, 185, 0, 52), function(state)
    if state then
        antiNightConn = RunService.Heartbeat:Connect(function()
            Lighting.ClockTime = 12
        end)
    else
        if antiNightConn then
            antiNightConn:Disconnect()
            antiNightConn = nil
        end
    end
end)

-- 5. Blood Moon
local bloodMoonSky = nil
createCheckboxCard("Blood Moon", UDim2.new(0, 0, 0, 104), function(state)
    if state then
        Lighting.ClockTime = 0
        Lighting.Ambient = Color3.fromRGB(150, 0, 0)
        Lighting.OutdoorAmbient = Color3.fromRGB(100, 0, 0)

        local currentSky = Lighting:FindFirstChildOfClass("Sky")
        if currentSky then currentSky:Destroy() end

        bloodMoonSky = Instance.new("Sky")
        bloodMoonSky.Name = "BloodMoonSky"
        bloodMoonSky.SkyboxBk = "rbxassetid://252765664"
        bloodMoonSky.SkyboxDn = "rbxassetid://252763780"
        bloodMoonSky.SkyboxFt = "rbxassetid://252763780"
        bloodMoonSky.SkyboxLf = "rbxassetid://252763780"
        bloodMoonSky.SkyboxRt = "rbxassetid://252763780"
        bloodMoonSky.SkyboxUp = "rbxassetid://252765664"
        bloodMoonSky.Parent = Lighting
    else
        Lighting.ClockTime = originalLighting.ClockTime
        Lighting.Ambient = originalLighting.Ambient
        Lighting.OutdoorAmbient = originalLighting.OutdoorAmbient

        if Lighting:FindFirstChild("BloodMoonSky") then
            Lighting.BloodMoonSky:Destroy()
        end
        if originalLighting.Sky then
            originalLighting.Sky:Clone().Parent = Lighting
        end
    end
end)

-- 6. Ultara 8k
local ultraBlur = nil
local ultraColor = nil
createCheckboxCard("Ultara 8k", UDim2.new(0, 185, 0, 104), function(state)
    if state then
        settings().Rendering.QualityLevel = Enum.QualityLevel.Level21
        
        ultraBlur = Instance.new("DepthOfFieldEffect")
        ultraBlur.Name = "Ultra8kDOF"
        ultraBlur.FarIntensity = 0.1
        ultraBlur.FocusDistance = 0.05
        ultraBlur.InFocusRadius = 30
        ultraBlur.NearIntensity = 0.1
        ultraBlur.Parent = Lighting

        ultraColor = Instance.new("ColorCorrectionEffect")
        ultraColor.Name = "Ultra8kColor"
        ultraColor.Brightness = 0.05
        ultraColor.Contrast = 0.2
        ultraColor.Saturation = 0.25
        ultraColor.Parent = Lighting
    else
        settings().Rendering.QualityLevel = Enum.QualityLevel.Automatic
        if Lighting:FindFirstChild("Ultra8kDOF") then Lighting.Ultra8kDOF:Destroy() end
        if Lighting:FindFirstChild("Ultra8kColor") then Lighting.Ultra8kColor:Destroy() end
    end
end)

-- Bottom Bar Components
BottomBar = Instance.new("Frame")
BottomBar.Parent = MainFrame
BottomBar.Size = UDim2.new(0, 345, 0, 32)
BottomBar.Position = UDim2.new(0, 140, 1, -40)
BottomBar.BackgroundTransparency = 1
BottomBar.Visible = false

local FpsBox = Instance.new("Frame")
FpsBox.Parent = BottomBar
FpsBox.Size = UDim2.new(0, 95, 1, 0)
FpsBox.Position = UDim2.new(0, 0, 0, 0)
FpsBox.BackgroundColor3 = Color3.fromRGB(245, 245, 245)

local FpsCorner = Instance.new("UICorner")
FpsCorner.CornerRadius = UDim.new(0, 8)
FpsCorner.Parent = FpsBox

local FpsStroke = Instance.new("UIStroke")
FpsStroke.Color = Color3.fromRGB(0, 0, 0)
FpsStroke.Thickness = 1.5
FpsStroke.Parent = FpsBox

local FpsLabel = Instance.new("TextLabel")
FpsLabel.Parent = FpsBox
FpsLabel.Size = UDim2.new(1, 0, 1, 0)
FpsLabel.BackgroundTransparency = 1
FpsLabel.Text = "Fps18"
FpsLabel.TextColor3 = Color3.fromRGB(0, 0, 0)
FpsLabel.Font = Enum.Font.SourceSansBold
FpsLabel.TextSize = 16

local ClockBox = Instance.new("Frame")
ClockBox.Parent = BottomBar
ClockBox.Size = UDim2.new(0, 95, 1, 0)
ClockBox.Position = UDim2.new(0, 105, 0, 0)
ClockBox.BackgroundColor3 = Color3.fromRGB(245, 245, 245)

local ClockCorner = Instance.new("UICorner")
ClockCorner.CornerRadius = UDim.new(0, 8)
ClockCorner.Parent = ClockBox

local ClockStroke = Instance.new("UIStroke")
ClockStroke.Color = Color3.fromRGB(0, 0, 0)
ClockStroke.Thickness = 1.5
ClockStroke.Parent = ClockBox

local ClockLabel = Instance.new("TextLabel")
ClockLabel.Parent = ClockBox
ClockLabel.Size = UDim2.new(1, 0, 1, 0)
ClockLabel.BackgroundTransparency = 1
ClockLabel.Text = "17:00"
ClockLabel.TextColor3 = Color3.fromRGB(0, 0, 0)
ClockLabel.Font = Enum.Font.SourceSansBold
ClockLabel.TextSize = 16

local StatusBox = Instance.new("Frame")
StatusBox.Parent = BottomBar
StatusBox.Size = UDim2.new(0, 130, 1, 0)
StatusBox.Position = UDim2.new(0, 210, 0, 0)
StatusBox.BackgroundColor3 = Color3.fromRGB(245, 245, 245)

local StatusCorner = Instance.new("UICorner")
StatusCorner.CornerRadius = UDim.new(0, 8)
StatusCorner.Parent = StatusBox

local StatusStroke = Instance.new("UIStroke")
StatusStroke.Color = Color3.fromRGB(0, 0, 0)
StatusStroke.Thickness = 1.5
StatusStroke.Parent = StatusBox

local CrownIcon = Instance.new("ImageLabel")
CrownIcon.Parent = StatusBox
CrownIcon.Size = UDim2.new(0, 30, 0, 30)
CrownIcon.Position = UDim2.new(0, 5, 0.5, -15)
CrownIcon.BackgroundTransparency = 1
CrownIcon.Image = "rbxassetid://96843104543422"

local StatusLabel = Instance.new("TextLabel")
StatusLabel.Parent = StatusBox
StatusLabel.Size = UDim2.new(1, -38, 1, 0)
StatusLabel.Position = UDim2.new(0, 35, 0, 0)
StatusLabel.BackgroundTransparency = 1
StatusLabel.Font = Enum.Font.SourceSansBold
StatusLabel.TextSize = 16

if LocalPlayer.Name == "LifzxAfter2" then
    StatusLabel.Text = "PREMIUM"
    StatusLabel.TextColor3 = Color3.fromRGB(255, 170, 0)
else
    StatusLabel.Text = "FREE"
    StatusLabel.TextColor3 = Color3.fromRGB(0, 0, 0)
end

local frameCount = 0
local lastFpsUpdate = os.clock()

RunService.RenderStepped:Connect(function()
    frameCount = frameCount + 1
    local now = os.clock()
    if now - lastFpsUpdate >= 1 then
        local currentFps = math.floor(frameCount / (now - lastFpsUpdate))
        FpsLabel.Text = "Fps" .. tostring(currentFps)
        frameCount = 0
        lastFpsUpdate = now
    end
    ClockLabel.Text = os.date("%H:%M")
end)

local FloatingBtn = Instance.new("ImageButton")
FloatingBtn.Name = "FloatingBannerBtn"
FloatingBtn.Parent = StealAnEggUI
FloatingBtn.Size = UDim2.new(0, 60, 0, 60)
FloatingBtn.Position = UDim2.new(0, 15, 0.5, -30)
FloatingBtn.Image = "rbxassetid://134683827360822"
FloatingBtn.BackgroundTransparency = 0.5
FloatingBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
FloatingBtn.Visible = false
FloatingBtn.Active = true
FloatingBtn.Draggable = true

local FloatCorner = Instance.new("UICorner")
FloatCorner.CornerRadius = UDim.new(0, 12)
FloatCorner.Parent = FloatingBtn

local RainbowStroke = Instance.new("UIStroke")
RainbowStroke.Parent = FloatingBtn
RainbowStroke.Thickness = 3
RainbowStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

local hue = 0
RunService.RenderStepped:Connect(function(delta)
    hue = (hue + delta * 0.5) % 1
    local rainbowColor = Color3.fromHSV(hue, 1, 1)
    RainbowStroke.Color = rainbowColor
    OpenScriptBtn.TextColor3 = rainbowColor
end)

MinBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    FloatingBtn.Visible = true
end)

FloatingBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = true
    FloatingBtn.Visible = false
end)

CloseBtn.MouseButton1Click:Connect(function()
    StealAnEggUI:Destroy()
end)

-------------------------------------------------------------------------
-- NOTIFIKASI GLOBAL & PRIVAT LZY HUB
-------------------------------------------------------------------------

local function createRainbowBorder(parent)
    local stroke = Instance.new("UIStroke")
    stroke.Thickness = 2.5
    stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
    stroke.Parent = parent
    
    task.spawn(function()
        local h = 0
        while parent and parent.Parent do
            h = (h + 0.02) % 1
            stroke.Color = Color3.fromHSV(h, 1, 1)
            task.wait()
        end
    end)
    
    return stroke
end

-- User ID Owner LifzxAfter2
local OWNER_NAME = "LifzxAfter2"
local OWNER_USER_ID = 6989405626

task.spawn(function()
    pcall(function()
        OWNER_USER_ID = Players:GetUserIdFromNameAsync(OWNER_NAME)
    end)
end)

-- Function Menampilkan Pop-Up Notifikasi LZY HUB
local function showLzyNotification(msg)
    local oldNotif = CoreGui:FindFirstChild("LzyHubNotif")
    if oldNotif then oldNotif:Destroy() end

    local NotifGui = Instance.new("ScreenGui")
    NotifGui.Name = "LzyHubNotif"
    NotifGui.Parent = CoreGui
    NotifGui.ResetOnSpawn = false
    NotifGui.IgnoreGuiInset = true

    local NotifFrame = Instance.new("Frame")
    NotifFrame.Parent = NotifGui
    NotifFrame.Size = UDim2.new(0, 450, 0, 180)
    NotifFrame.Position = UDim2.new(0.5, -225, 0.5, -90)
    NotifFrame.BackgroundColor3 = Color3.fromRGB(120, 120, 120)
    NotifFrame.BorderSizePixel = 0

    local NotifCorner = Instance.new("UICorner")
    NotifCorner.CornerRadius = UDim.new(0, 15)
    NotifCorner.Parent = NotifFrame

    -- Foto Profil Bulat LifzxAfter2
    local NotifAvatar = Instance.new("ImageLabel")
    NotifAvatar.Parent = NotifFrame
    NotifAvatar.Size = UDim2.new(0, 70, 0, 70)
    NotifAvatar.Position = UDim2.new(0, 20, 0, 20)
    NotifAvatar.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    NotifAvatar.BackgroundTransparency = 1

    local AvCorner = Instance.new("UICorner")
    AvCorner.CornerRadius = UDim.new(1, 0)
    AvCorner.Parent = NotifAvatar

    task.spawn(function()
        local content, isReady = Players:GetUserThumbnailAsync(OWNER_USER_ID, Enum.ThumbnailType.HeadShot, Enum.ThumbnailSize.Size420x420)
        if isReady then 
            NotifAvatar.Image = content 
        end
    end)

    -- Judul LZY HUB + Rainbow Outline
    local TitleLabel = Instance.new("TextLabel")
    TitleLabel.Parent = NotifFrame
    TitleLabel.Size = UDim2.new(0, 280, 0, 40)
    TitleLabel.Position = UDim2.new(0, 100, 0, 15)
    TitleLabel.BackgroundTransparency = 1
    TitleLabel.Text = "LZY HUB"
    TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    TitleLabel.Font = Enum.Font.GothamBlack
    TitleLabel.TextSize = 32
    TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
    
    local titleStroke = Instance.new("UIStroke")
    titleStroke.Color = Color3.fromRGB(255, 255, 255)
    titleStroke.Thickness = 1.5
    titleStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Contextual
    titleStroke.Parent = TitleLabel
    
    task.spawn(function()
        local h = 0
        while TitleLabel and TitleLabel.Parent do
            h = (h + 0.03) % 1
            titleStroke.Color = Color3.fromHSV(h, 1, 1)
            task.wait()
        end
    end)

    -- Icon Mahkota
    local NotifCrown = Instance.new("ImageLabel")
    NotifCrown.Parent = NotifFrame
    NotifCrown.Size = UDim2.new(0, 40, 0, 40)
    NotifCrown.Position = UDim2.new(0, 380, 0, 10)
    NotifCrown.BackgroundTransparency = 1
    NotifCrown.Image = "rbxassetid://96843104543422"

    -- Teks [OWNER] dengan Pinggiran Hitam
    local OwnerTag = Instance.new("TextLabel")
    OwnerTag.Parent = NotifFrame
    OwnerTag.Size = UDim2.new(0, 120, 0, 30)
    OwnerTag.Position = UDim2.new(0, 100, 0, 55)
    OwnerTag.BackgroundTransparency = 1
    OwnerTag.Text = "[OWNER]"
    OwnerTag.TextColor3 = Color3.fromRGB(255, 0, 0)
    OwnerTag.Font = Enum.Font.GothamBlack
    OwnerTag.TextSize = 24
    OwnerTag.TextXAlignment = Enum.TextXAlignment.Left
    
    local ownerStroke = Instance.new("UIStroke")
    ownerStroke.Color = Color3.fromRGB(0, 0, 0)
    ownerStroke.Thickness = 2
    ownerStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Contextual
    ownerStroke.Parent = OwnerTag

    -- Pesan Notifikasi
    local MsgLabel = Instance.new("TextLabel")
    MsgLabel.Parent = NotifFrame
    MsgLabel.Size = UDim2.new(1, -40, 0, 50)
    MsgLabel.Position = UDim2.new(0, 20, 0, 100)
    MsgLabel.BackgroundTransparency = 1
    MsgLabel.Text = "PESAN : " .. msg
    MsgLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    MsgLabel.Font = Enum.Font.GothamBlack
    MsgLabel.TextSize = 24
    MsgLabel.TextXAlignment = Enum.TextXAlignment.Left
    MsgLabel.TextWrapped = true
    
    local msgStroke = Instance.new("UIStroke")
    msgStroke.Color = Color3.fromRGB(0, 0, 0)
    msgStroke.Thickness = 2
    msgStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Contextual
    msgStroke.Parent = MsgLabel

    -- Animasi Masuk
    NotifFrame.Position = UDim2.new(0.5, -225, 0.5, -120)
    NotifFrame.BackgroundTransparency = 1
    TitleLabel.TextTransparency = 1
    OwnerTag.TextTransparency = 1
    MsgLabel.TextTransparency = 1
    NotifAvatar.ImageTransparency = 1
    NotifCrown.ImageTransparency = 1

    TweenService:Create(NotifFrame, TweenInfo.new(0.4, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
        Position = UDim2.new(0.5, -225, 0.5, -90),
        BackgroundTransparency = 0
    }):Play()
    
    TweenService:Create(TitleLabel, TweenInfo.new(0.4), {TextTransparency = 0}):Play()
    TweenService:Create(OwnerTag, TweenInfo.new(0.5), {TextTransparency = 0}):Play()
    TweenService:Create(MsgLabel, TweenInfo.new(0.6), {TextTransparency = 0}):Play()
    TweenService:Create(NotifAvatar, TweenInfo.new(0.3), {ImageTransparency = 0}):Play()
    TweenService:Create(NotifCrown, TweenInfo.new(0.4), {ImageTransparency = 0}):Play()

    -- Hilang setelah 6 detik
    task.delay(6, function()
        if NotifGui and NotifGui.Parent then
            TweenService:Create(NotifFrame, TweenInfo.new(0.3), {BackgroundTransparency = 1}):Play()
            task.wait(0.3)
            NotifGui:Destroy()
        end
    end)
end


-- === SISTEM REALTIME BROADCAST DENGAN RESTFUL CLOUD DB (100% TERKIRIM KE SEMUA PLAYER) ===
local req = (syn and syn.request) or (http and http.request) or http_request or (fluxus and fluxus.request) or request
local DATABASE_URL = "https://api.restful-api.dev/objects/ff808181932 bad12019323381a1a0000" -- Public REST Database ID Khusus LZY HUB

local lastMessageID = ""
local initialLoad = true

-- Master HttpRequest Wrapper (Support All Executor)
local function executeHttpRequest(options)
    if req then
        return req(options)
    elseif game and game.HttpGet then
        if options.Method == "GET" then
            local res = game:HttpGet(options.Url)
            return {Body = res, StatusCode = 200}
        end
    end
    return nil
end

-- Polling Otomatis (Setiap 2.5 detik mengecek pesan baru dari server)
task.spawn(function()
    while task.wait(2.5) do
        pcall(function()
            local response = executeHttpRequest({
                Url = "https://api.restful-api.dev/objects/ff808181932bad12019323381a1a0000",
                Method = "GET"
            })
            
            if response and response.Body then
                local decoded = HttpService:JSONDecode(response.Body)
                if decoded and decoded.data and decoded.data.broadcast then
                    local currentMsg = decoded.data.broadcast
                    local currentTimestamp = decoded.data.time or ""

                    -- Deteksi pesan baru unik
                    local uniqueKey = currentMsg .. "_" .. tostring(currentTimestamp)
                    if uniqueKey ~= lastMessageID then
                        lastMessageID = uniqueKey
                        
                        -- Jangan memunculkan notifikasi saat baru pertama kali script dieksekusi jika bukan pesan baru
                        if not initialLoad then
                            showLzyNotification(currentMsg)
                        end
                    end
                end
            end
        end)
        initialLoad = false
    end
end)

-- Function Mengirim Pesan Broadcast Lintas Server Ke Semua Player LZY HUB
local function sendGlobalBroadcast(message)
    task.spawn(function()
        pcall(function()
            local payload = HttpService:JSONEncode({
                name = "LZY_HUB_GLOBAL_BROADCAST",
                data = {
                    broadcast = message,
                    sender = LocalPlayer.Name,
                    time = os.time()
                }
            })

            executeHttpRequest({
                Url = "https://api.restful-api.dev/objects/ff808181932bad12019323381a1a0000",
                Method = "PUT",
                Headers = {
                    ["Content-Type"] = "application/json"
                },
                Body = payload
            })
        end)
    end)
end


-- === ADMIN TAB INTERFACE ===

if IS_OWNER then
    local ModeFrame = Instance.new("Frame")
    ModeFrame.Parent = AdminTabContainer
    ModeFrame.Size = UDim2.new(1, 0, 0, 45)
    ModeFrame.Position = UDim2.new(0, 0, 0, 10)
    ModeFrame.BackgroundTransparency = 1

    local currentMode = "Privat"

    local btnPrivat = Instance.new("TextButton")
    btnPrivat.Parent = ModeFrame
    btnPrivat.Size = UDim2.new(0.48, 0, 1, 0)
    btnPrivat.Position = UDim2.new(0, 0, 0, 0)
    btnPrivat.BackgroundColor3 = Color3.fromRGB(245, 245, 245)
    btnPrivat.Text = "Privat"
    btnPrivat.TextColor3 = Color3.fromRGB(0, 0, 0)
    btnPrivat.Font = Enum.Font.GothamBlack
    btnPrivat.TextSize = 20
    
    local privCorner = Instance.new("UICorner")
    privCorner.CornerRadius = UDim.new(0, 10)
    privCorner.Parent = btnPrivat
    
    local privStroke = Instance.new("UIStroke")
    privStroke.Thickness = 2.5
    privStroke.Color = Color3.fromRGB(255, 255, 255)
    privStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
    privStroke.Parent = btnPrivat
    
    task.spawn(function()
        local h = 0
        while btnPrivat and btnPrivat.Parent do
            if currentMode == "Privat" then
                h = (h + 0.03) % 1
                privStroke.Color = Color3.fromHSV(h, 1, 1)
            else
                privStroke.Color = Color3.fromRGB(150, 150, 150)
            end
            task.wait()
        end
    end)

    local btnGlobal = Instance.new("TextButton")
    btnGlobal.Parent = ModeFrame
    btnGlobal.Size = UDim2.new(0.48, 0, 1, 0)
    btnGlobal.Position = UDim2.new(0.52, 0, 0, 0)
    btnGlobal.BackgroundColor3 = Color3.fromRGB(245, 245, 245)
    btnGlobal.Text = "Global"
    btnGlobal.TextColor3 = Color3.fromRGB(0, 0, 0)
    btnGlobal.Font = Enum.Font.GothamBlack
    btnGlobal.TextSize = 20
    
    local globCorner = Instance.new("UICorner")
    globCorner.CornerRadius = UDim.new(0, 10)
    globCorner.Parent = btnGlobal
    
    local globStroke = Instance.new("UIStroke")
    globStroke.Thickness = 2.5
    globStroke.Color = Color3.fromRGB(150, 150, 150)
    globStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
    globStroke.Parent = btnGlobal
    
    task.spawn(function()
        local h = 0
        while btnGlobal and btnGlobal.Parent do
            if currentMode == "Global" then
                h = (h + 0.03) % 1
                globStroke.Color = Color3.fromHSV(h, 1, 1)
            else
                globStroke.Color = Color3.fromRGB(150, 150, 150)
            end
            task.wait()
        end
    end)

    btnPrivat.MouseButton1Click:Connect(function()
        currentMode = "Privat"
    end)

    btnGlobal.MouseButton1Click:Connect(function()
        currentMode = "Global"
    end)

    local InputBox = Instance.new("TextBox")
    InputBox.Parent = AdminTabContainer
    InputBox.Size = UDim2.new(1, -20, 0, 45)
    InputBox.Position = UDim2.new(0, 10, 0, 70)
    InputBox.BackgroundColor3 = Color3.fromRGB(245, 245, 245)
    InputBox.PlaceholderText = "Pesan..."
    InputBox.PlaceholderColor3 = Color3.fromRGB(100, 100, 100)
    InputBox.Text = ""
    InputBox.TextColor3 = Color3.fromRGB(0, 0, 0)
    InputBox.Font = Enum.Font.GothamBold
    InputBox.TextSize = 20
    InputBox.ClearTextOnFocus = false
    
    local inputCorner = Instance.new("UICorner")
    inputCorner.CornerRadius = UDim.new(0, 10)
    inputCorner.Parent = InputBox
    
    local inputStroke = Instance.new("UIStroke")
    inputStroke.Color = Color3.fromRGB(0, 0, 0)
    inputStroke.Thickness = 1.5
    inputStroke.Parent = InputBox

    local SendBtn = Instance.new("TextButton")
    SendBtn.Parent = AdminTabContainer
    SendBtn.Size = UDim2.new(0.4, 0, 0, 45)
    SendBtn.Position = UDim2.new(0.3, 0, 0, 130)
    SendBtn.BackgroundColor3 = Color3.fromRGB(245, 245, 245)
    SendBtn.Text = "SEND"
    SendBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
    SendBtn.Font = Enum.Font.GothamBlack
    SendBtn.TextSize = 22
    
    local sendCorner = Instance.new("UICorner")
    sendCorner.CornerRadius = UDim.new(0, 10)
    sendCorner.Parent = SendBtn
    
    local sendStroke = Instance.new("UIStroke")
    sendStroke.Color = Color3.fromRGB(0, 0, 0)
    sendStroke.Thickness = 1.5
    sendStroke.Parent = SendBtn

    -- Tombol SEND Click Listener
    SendBtn.MouseButton1Click:Connect(function()
        local msg = InputBox.Text
        if msg and msg:gsub("%s+", "") ~= "" then
            if currentMode == "Global" then
                -- Kirim broadcast lintas server secara online ke semua player LZY HUB
                sendGlobalBroadcast(msg)
            else
                -- Tampilkan khusus privat (diri sendiri saja)
                showLzyNotification(msg)
            end
            InputBox.Text = ""
        end
    end)

else
    -- Tampilan untuk Non-Owner
    local UnlockFrame = Instance.new("Frame")
    UnlockFrame.Parent = AdminTabContainer
    UnlockFrame.Size = UDim2.new(0, 250, 0, 60)
    UnlockFrame.Position = UDim2.new(0.5, -125, 0.5, -30)
    UnlockFrame.BackgroundColor3 = Color3.fromRGB(245, 245, 245)
    
    local unlockCorner = Instance.new("UICorner")
    unlockCorner.CornerRadius = UDim.new(0, 12)
    unlockCorner.Parent = UnlockFrame
    
    createRainbowBorder(UnlockFrame)
    
    local UnlockLabel = Instance.new("TextLabel")
    UnlockLabel.Parent = UnlockFrame
    UnlockLabel.Size = UDim2.new(1, 0, 1, 0)
    UnlockLabel.BackgroundTransparency = 1
    UnlockLabel.Text = "UNLOCK"
    UnlockLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    UnlockLabel.Font = Enum.Font.GothamBlack
    UnlockLabel.TextSize = 28
    
    local unlockTextStroke = Instance.new("UIStroke")
    unlockTextStroke.Color = Color3.fromRGB(255, 255, 255)
    unlockTextStroke.Thickness = 2
    unlockTextStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Contextual
    unlockTextStroke.Parent = UnlockLabel
    
    task.spawn(function()
        local h = 0
        while UnlockLabel and UnlockLabel.Parent do
            h = (h + 0.03) % 1
            unlockTextStroke.Color = Color3.fromHSV(h, 1, 1)
            task.wait()
        end
    end)
end
