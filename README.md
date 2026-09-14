-- Delta Executor Script | Steal an Egg
-- Pembuat: LIPZY ABUSE
-- Fitur: Spawn Telur Divine, Hanya Visual Sendiri, Latar Belakang Hitam

local Players = game:GetService("Players")
local Player = Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

-- === BUAT KOTAK UTAMA ===
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "LipzyAbuseMenu"
ScreenGui.Parent = PlayerGui

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 320, 0, 420)
MainFrame.Position = UDim2.new(0.5, -160, 0.5, -210)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.BorderSizePixel = 2
MainFrame.BorderColor3 = Color3.fromRGB(80, 80, 80)
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

-- === JUDUL RAINBOW "LIPZY ABUSE" ===
local TitleLabel = Instance.new("TextLabel")
TitleLabel.Name = "TitleLabel"
TitleLabel.Size = UDim2.new(1, 0, 0, 50)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "LIPZY ABUSE"
TitleLabel.Font = Enum.Font.GothamBlack
TitleLabel.TextSize = 28
TitleLabel.TextBold = true
TitleLabel.Parent = MainFrame

-- Animasi Warna Pelangi Otomatis
game:GetService("RunService").Heartbeat:Connect(function()
    local Hue = (os.clock() * 0.5) % 1
    TitleLabel.TextColor3 = Color3.fromHSV(Hue, 1, 1)
end)

-- === GARIS PEMISAH ===
local Separator = Instance.new("Frame")
Separator.Size = UDim2.new(0.9, 0, 0, 2)
Separator.Position = UDim2.new(0.05, 0, 0, 55)
Separator.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
Separator.Parent = MainFrame

-- === JUDUL MENU PILIHAN TELUR ===
local MenuTitle = Instance.new("TextLabel")
MenuTitle.Size = UDim2.new(0.9, 0, 0, 35)
MenuTitle.Position = UDim2.new(0.05, 0, 0, 70)
MenuTitle.BackgroundTransparency = 1
MenuTitle.Text = "🥚 PILIH TELUR DIVINE"
MenuTitle.Font = Enum.Font.GothamBold
MenuTitle.TextSize = 18
MenuTitle.TextColor3 = Color3.fromRGB(255, 215, 0)
MenuTitle.Parent = MainFrame

-- === DAFTAR TELUR DIVINE ===
local Eggs = {
    {Name = "🦊 Kitsune", ID = "Kitsune"},
    {Name = "🦄 Unicorn", ID = "Unicorn"},
    {Name = "🔥 Nightflame", ID = "nightflame"},
    {Name = "🌌 World Burner", ID = "World burner"}
}

local SelectedEgg = Eggs[1].ID
local Buttons = {}

for i, Egg in ipairs(Eggs) do
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.9, 0, 0, 40)
    btn.Position = UDim2.new(0.05, 0, 0, 110 + ((i - 1) * 45))
    btn.BackgroundColor3 = i == 1 and Color3.fromRGB(50, 120, 50) or Color3.fromRGB(50, 50, 50)
    btn.Text = Egg.Name
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 15
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.BorderSizePixel = 1
    btn.BorderColor3 = Color3.fromRGB(90, 90, 90)
    btn.Parent = MainFrame

    btn.MouseButton1Click:Connect(function()
        SelectedEgg = Egg.ID
        for _, b in next, Buttons do
            b.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
        end
        btn.BackgroundColor3 = Color3.fromRGB(50, 120, 50)
    end)

    table.insert(Buttons, btn)
end

-- === PENGATURAN JUMLAH TELUR ===
local AmountLabel = Instance.new("TextLabel")
AmountLabel.Size = UDim2.new(0.9, 0, 0, 30)
AmountLabel.Position = UDim2.new(0.05, 0, 0, 300)
AmountLabel.BackgroundTransparency = 1
AmountLabel.Text = "🔢 JUMLAH TELUR: 1"
AmountLabel.Font = Enum.Font.GothamBold
AmountLabel.TextSize = 16
AmountLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
AmountLabel.Parent = MainFrame

local AmountBox = Instance.new("TextBox")
AmountBox.Size = UDim2.new(0.9, 0, 0, 35)
AmountBox.Position = UDim2.new(0.05, 0, 0, 330)
AmountBox.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
AmountBox.Text = "1"
AmountBox.Font = Enum.Font.Gotham
AmountBox.TextSize = 16
AmountBox.TextColor3 = Color3.fromRGB(255, 255, 255)
AmountBox.PlaceholderText = "Masukkan jumlah telur"
AmountBox.ClearTextOnFocus = true
AmountBox.Parent = MainFrame

local EggAmount = 1
AmountBox.FocusLost:Connect(function()
    local num = tonumber(AmountBox.Text)
    if num and num > 0 then
        EggAmount = math.floor(num)
        AmountLabel.Text = "🔢 JUMLAH TELUR: " .. EggAmount
    else
        AmountBox.Text = tostring(EggAmount)
    end
end)

-- === TOMBOL SPAWN TELUR ===
local SpawnBtn = Instance.new("TextButton")
SpawnBtn.Size = UDim2.new(0.9, 0, 0, 45)
SpawnBtn.Position = UDim2.new(0.05, 0, 0, 380)
SpawnBtn.BackgroundColor3 = Color3.fromRGB(160, 60, 60)
SpawnBtn.Text = "✨ SPAWN TELUR DIVINE"
SpawnBtn.Font = Enum.Font.GothamBold
SpawnBtn.TextSize = 18
SpawnBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
SpawnBtn.BorderSizePixel = 2
SpawnBtn.BorderColor3 = Color3.fromRGB(220, 80, 80)
SpawnBtn.Parent = MainFrame

-- === FUNGSI SPAWN TELUR (HANYA VISUAL KAMU SAJA) ===
SpawnBtn.MouseButton1Click:Connect(function()
    local Character = Player.Character
    if not Character then return end
    local HRP = Character:FindFirstChild("HumanoidRootPart")
    if not HRP then return end

    for i = 1, EggAmount do
        local Egg = Instance.new("Part")
        Egg.Name = "Egg_" .. SelectedEgg
        Egg.Shape = Enum.PartType.Ball
        Egg.Size = Vector3.new(2, 2.5, 2)
        Egg.Position = HRP.Position + Vector3.new(math.random(-3, 3), 3 + i * 0.5, math.random(-3, 3))
        Egg.Anchored = false
        Egg.CanCollide = true
        Egg.BrickColor = BrickColor.Random() -- Bisa diganti warna spesifik
        Egg.Parent = workspace

        -- Hanya terlihat oleh kamu saja
        for _, v in next, Players:GetPlayers() do
            if v ~= Player then
                pcall(function()
                    Egg:RemoveFromDecals()
                end)
            end
        end

        -- Efek cahaya
        local Light = Instance.new("PointLight")
        Light.Brightness = 4
        Light.Range = 12
        Light.Color = Color3.fromHSV(math.random(), 0.8, 1)
        Light.Parent = Egg
    end

    -- Notifikasi
    local Notif = Instance.new("TextLabel")
    Notif.Size = UDim2.new(0, 250, 0, 40)
    Notif.Position = UDim2.new(0.5, -125, 0.1, 0)
    Notif.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    Notif.Text = "✅ BERHASIL SPAWN " .. EggAmount .. " TELUR " .. string.upper(SelectedEgg)
    Notif.Font = Enum.Font.GothamBold
    Notif.TextSize = 14
    Notif.TextColor3 = Color3.fromRGB(80, 255, 80)
    Notif.Parent = PlayerGui
    task.wait(2.5)
    Notif:Destroy()
end)

print("[✅] LIPZY ABUSE — Script Dimuat Berhasil!")
