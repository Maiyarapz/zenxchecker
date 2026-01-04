-- [[ MONITOR UI WITH PROFILE IMAGE & HIDE SYSTEM ]] --
-- สำหรับคุณ z3xn_

local HttpService = game:GetService("HttpService")
local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")
local request = (syn and syn.request) or (http and http.request) or http_request or (fluxus and fluxus.request) or request

-- 📁 ระบบบันทึกข้อมูล
local fileName = "z3xn_Monitor_Config.json"
local config = { webhook = "", discordId = "" }

local function saveConfig()
    writefile(fileName, HttpService:JSONEncode(config))
end

local function loadConfig()
    if isfile(fileName) then
        pcall(function() config = HttpService:JSONDecode(readfile(fileName)) end)
    end
end
loadConfig()

-- 🖥️ สร้าง ScreenGui
local ScreenGui = Instance.new("ScreenGui", CoreGui)
ScreenGui.Name = "MonitorGui_z3xn"

-- 🖼️ ปุ่มเปิด/ปิด แบบมีรูปโปรไฟล์ (Toggle Button)
local ToggleBtn = Instance.new("ImageButton", ScreenGui)
ToggleBtn.Size = UDim2.new(0, 60, 0, 60)
ToggleBtn.Position = UDim2.new(0, 15, 0.5, -30)
ToggleBtn.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
ToggleBtn.BorderSizePixel = 2
ToggleBtn.BorderColor3 = Color3.fromRGB(0, 170, 255)
-- ดึงรูปหน้าตัวละครของคุณมาเป็นปุ่ม
ToggleBtn.Image = "https://www.roblox.com/headshot-thumbnail/image?userId=" .. Players.LocalPlayer.UserId .. "&width=420&height=420&format=png"

local cornerBtn = Instance.new("UICorner", ToggleBtn)
cornerBtn.CornerRadius = UDim.new(1, 0) -- ทำให้ปุ่มกลม

-- 🖼️ หน้าต่างหลัก (Main Frame)
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 320, 0, 260)
MainFrame.Position = UDim2.new(0.5, -160, 0.5, -130)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.Visible = true
MainFrame.Active = true
MainFrame.Draggable = true
Instance.new("UICorner", MainFrame)

local Title = Instance.new("TextLabel", MainFrame)
Title.Size = UDim2.new(1, 0, 0, 45)
Title.Text = "🛡️ MONITOR CONFIG"
Title.TextColor3 = Color3.new(1, 1, 1)
Title.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 20
Instance.new("UICorner", Title)

-- ช่องใส่ Webhook
local WebhookInput = Instance.new("TextBox", MainFrame)
WebhookInput.PlaceholderText = "Paste Discord Webhook URL..."
WebhookInput.Text = config.webhook
WebhookInput.Size = UDim2.new(0.9, 0, 0, 45)
WebhookInput.Position = UDim2.new(0.05, 0, 0.25, 0)
WebhookInput.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
WebhookInput.TextColor3 = Color3.new(1, 1, 1)
Instance.new("UICorner", WebhookInput)

-- ช่องใส่ Discord ID
local IDInput = Instance.new("TextBox", MainFrame)
IDInput.PlaceholderText = "Enter Discord ID (e.g. 45863...)"
IDInput.Text = config.discordId
IDInput.Size = UDim2.new(0.9, 0, 0, 45)
IDInput.Position = UDim2.new(0.05, 0, 0.5, 0)
IDInput.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
IDInput.TextColor3 = Color3.new(1, 1, 1)
Instance.new("UICorner", IDInput)

-- ปุ่ม Save & Start
local StartBtn = Instance.new("TextButton", MainFrame)
StartBtn.Text = "🚀 START MONITORING"
StartBtn.Size = UDim2.new(0.9, 0, 0, 55)
StartBtn.Position = UDim2.new(0.05, 0, 0.75, 0)
StartBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 215)
StartBtn.TextColor3 = Color3.new(1, 1, 1)
StartBtn.Font = Enum.Font.SourceSansBold
StartBtn.TextSize = 18
Instance.new("UICorner", StartBtn)

-----------------------------------------------------------
-- ⚙️ ระบบแจ้งเตือน
-----------------------------------------------------------
local function sendNotify(player, statusType)
    if config.webhook == "" or not config.webhook:find("discord") then return end
    
    local isMe = (player == Players.LocalPlayer)
    local title = statusType == "Join" and "📥 PLAYER JOINED" or "📤 PLAYER LEFT"
    local color = statusType == "Join" and 65280 or 16776960
    if isMe and statusType == "Leave" then 
        title = "🚨 DISCONNECTED (YOU)"
        color = 16711680 
    end

    local payload = {
        ["content"] = "<@" .. config.discordId .. ">",
        ["embeds"] = {{
            ["title"] = title,
            ["color"] = color,
            ["thumbnail"] = { ["url"] = "https://www.roblox.com/headshot-thumbnail/image?userId=" .. player.UserId .. "&width=420&height=420&format=png" },
            ["fields"] = {
                {["name"] = "👤 Player", ["value"] = player.Name, ["inline"] = true},
                {["name"] = "⏳ Age", ["value"] = player.AccountAge .. " days", ["inline"] = true}
            },
            ["timestamp"] = os.date("!%Y-%m-%dT%H:%M:%SZ")
        }}
    }

    pcall(function()
        request({
            Url = config.webhook,
            Method = "POST",
            Headers = {["Content-Type"] = "application/json"},
            Body = HttpService:JSONEncode(payload)
        })
    end)
end

-- สลับการเปิด/ปิดหน้าต่าง
ToggleBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
end)

-- เริ่มการทำงาน
local isRunning = false
StartBtn.MouseButton1Click:Connect(function()
    config.webhook = WebhookInput.Text
    config.discordId = IDInput.Text
    saveConfig()
    
    if not isRunning then
        isRunning = true
        Players.PlayerAdded:Connect(function(p) sendNotify(p, "Join") end)
        Players.PlayerRemoving:Connect(function(p) sendNotify(p, "Leave") end)
        StartBtn.Text = "🟢 MONITORING..."
        StartBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    end
    MainFrame.Visible = false
end)
