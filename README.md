loadstring([[
    function LPH_NO_VIRTUALIZE(f) return f end;
]])();

--[[
	WARNING: Heads up! This script has not been verified by ScriptBlox. Use at your own risk!
]]
-- [[ HDX HUB - BROOKHAVEN EDITION (تفعيل تلقائي) ]] 
--Alsaadi سكربت
Dark_Santos090 | روبلوكس: Dark_Santos090
-- الإصدار: V3.5 | النسخة الرسمية الشاملة

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- جلب بيانات المستخدم والنظام
local Player = game.Players.LocalPlayer
local DisplayName = Player.DisplayName
local Name = Player.Name
local UserID = Player.UserId
local Executor = identifyexecutor and identifyexecutor() or "Unknown"
local StartTime = tick()

-- ========== متغيرات الحماية ULTRA PRO MAX ==========
local UltraEnabled = false
local Connections = {}
local lastSafeCFrame = nil
local lastCheck = tick()
local MAX_SPEED = 85
local MAX_FALL_SPEED = -120
local MAP_Y_LIMIT = -80
local AntiKickEnabled = false
local AntiFreezeEnabled = false
local AntiCrashEnabled = false
local AntiLagEnabled = false

-- ========== متغيرات متحكم ألقاب RGB ==========
local isRainbowActive = false
local currentSpeedLevel = 5
local speedIntervals = {0.3, 0.2, 0.15, 0.1, 0.08, 0.05, 0.03, 0.02, 0.01, 0.005}

-- النصوص المنفصلة للقب والبيو الثابتة
local defaultTitleText = "≠ Alsaadi تـم تشـغيـل سكـربـت ≠"
local defaultBioText = "Dark_Santos090"
local isFirstRun = true

-- ========== دوال الحماية المتطورة ==========
local function disconnectAll()
    for _, c in ipairs(Connections) do
        pcall(function() c:Disconnect() end)
    end
    table.clear(Connections)
end

local function getChar() return game.Players.LocalPlayer.Character end
local function getHRP() 
    local c = getChar() 
    return c and c:FindFirstChild("HumanoidRootPart") 
end
local function getHum() 
    local c = getChar() 
    return c and c:FindFirstChildOfClass("Humanoid") 
end

local function restorePosition()
    local hrp = getHRP()
    if hrp and lastSafeCFrame then
        pcall(function()
            hrp.AssemblyLinearVelocity = Vector3.zero
            hrp.AssemblyAngularVelocity = Vector3.zero
            hrp.CFrame = lastSafeCFrame + Vector3.new(0, 5, 0)
        end)
    end
end

-- دالة الانتقال الخاصة بتبويب الإحداثيات
local function teleportTo(x, y, z)
    local hrp = getHRP()
    if hrp then
        hrp.CFrame = CFrame.new(x, y, z)
    end
end

-- ========== نظام منع الطرد المتقدم ==========
local function setupAntiKick()
    if not AntiKickEnabled then return end
    pcall(function()
        local player = game.Players.LocalPlayer
        if player then
            player.Kick = function() 
                if AntiKickEnabled then
                    warn("🚫 تم منع محاولة طرد!")
                    return 
                end
            end
        end
    end)
end

-- ========== نظام منع التجميد ==========
local setupAntiFreeze = LPH_NO_VIRTUALIZE(function()
    if not AntiFreezeEnabled then return end
    task.spawn(LPH_NO_VIRTUALIZE(function()
        while AntiFreezeEnabled do
            pcall(LPH_NO_VIRTUALIZE(function()
                local char = getChar()
                local hrp = getHRP()
                local hum = getHum()
                if char and hrp and hum then
                    if hrp.AssemblyLinearVelocity.Magnitude < 0.1 and hum:GetState() ~= Enum.HumanoidStateType.Seated then
                        hrp.AssemblyLinearVelocity = Vector3.new(0, 0, 0)
                    end
                    if hum:GetState() == Enum.HumanoidStateType.Freefall then
                        hum:ChangeState(Enum.HumanoidStateType.Landed)
                    end
                    if hum.WalkSpeed < 1 and hum.WalkSpeed > 0 then
                        hum.WalkSpeed = 16
                    end
                    if hum.JumpPower < 1 and hum.JumpPower > 0 then
                        hum.JumpPower = 50
                    end
                end
            end))
            task.wait(0.5)
        end
    end))
end)

-- ========== نظام منع الكراش ==========
local setupAntiCrash = LPH_NO_VIRTUALIZE(function()
    if not AntiCrashEnabled then return end
    task.spawn(LPH_NO_VIRTUALIZE(function()
        while AntiCrashEnabled do
            pcall(LPH_NO_VIRTUALIZE(function()
                for _, v in pairs(workspace:GetDescendants()) do
                    if (v:IsA("BasePart") or v:IsA("MeshPart")) and v.Transparency == 1 and v.Size.Magnitude > 100 then
                        v:Destroy()
                    end
                end
            end))
            task.wait(2)
        end
    end))
end)

-- ========== نظام منع اللاق ==========
local setupAntiLag = LPH_NO_VIRTUALIZE(function()
    if not AntiLagEnabled then return end
    task.spawn(LPH_NO_VIRTUALIZE(function()
        while AntiLagEnabled do
            pcall(LPH_NO_VIRTUALIZE(function()
                for _, v in pairs(workspace:GetDescendants()) do
                    if v:IsA("ParticleEmitter") then
                        v.Enabled = false
                        task.wait(0.1)
                        v.Enabled = true
                    elseif v:IsA("Trail") or v:IsA("Beam") or v:IsA("Smoke") then
                        v.Enabled = false
                    elseif v:IsA("Explosion") then
                        v:Destroy()
                    elseif v:IsA("Fire") then
                        v.Size = 0
                    end
                end
            end))
            task.wait(5)
        end
    end))
end)

-- ========== تفعيل دوال الحماية ULTRA PRO MAX ==========
local setupUltraProtection = LPH_NO_VIRTUALIZE(function()
    if not UltraEnabled then 
        disconnectAll()
        return 
    end
    
    disconnectAll()
    task.wait(0.4)

    local heartbeatConn = game:GetService("RunService").Heartbeat:Connect(LPH_NO_VIRTUALIZE(function()
        pcall(LPH_NO_VIRTUALIZE(function()
            local hrp = getHRP()
            if hrp then lastSafeCFrame = hrp.CFrame end
        end))
    end))
    table.insert(Connections, heartbeatConn)

    local heartbeatConn2 = game:GetService("RunService").Heartbeat:Connect(LPH_NO_VIRTUALIZE(function()
        if not UltraEnabled then return end
        pcall(LPH_NO_VIRTUALIZE(function()
            local char = getChar()
            local hrp = getHRP()
            local hum = getHum()
            if not (char and hrp and hum) then return end
            
            hum:SetStateEnabled(Enum.HumanoidStateType.Seated, false)
            if hum:GetState() == Enum.HumanoidStateType.Seated then
                hum:ChangeState(Enum.HumanoidStateType.GettingUp)
            end
            
            for _, p in ipairs(char:GetDescendants()) do
                if p:IsA("BasePart") then
                    p.CanCollide = true
                    p.Anchored = false
                end
            end
            
            hrp.AssemblyAngularVelocity = Vector3.zero
            if hrp.AssemblyLinearVelocity.Magnitude > MAX_SPEED then
                restorePosition()
            end
            hrp.CustomPhysicalProperties = PhysicalProperties.new(0.01, 0, 0, 0, 0)
        end))
    end))
    table.insert(Connections, heartbeatConn2)

    local heartbeatConn3 = game:GetService("RunService").Heartbeat:Connect(LPH_NO_VIRTUALIZE(function()
        if not UltraEnabled then return end
        pcall(LPH_NO_VIRTUALIZE(function()
            local hrp = getHRP()
            if not hrp then return end
            local now = tick()
            if now - lastCheck < 0.15 then return end
            lastCheck = now
            if hrp.AssemblyLinearVelocity.Y < MAX_FALL_SPEED or hrp.Position.Y < MAP_Y_LIMIT then
                restorePosition()
            end
        end))
    end))
    table.insert(Connections, heartbeatConn3)
    
    setupAntiKick()
    setupAntiFreeze()
    setupAntiCrash()
    setupAntiLag()
end)

-- ========== دالة تنظيف اللقب والبيو ==========
local function clearTitleAndBio()
    pcall(function()
        local RE = game:GetService("ReplicatedStorage"):WaitForChild("RE")
        local TextRemote = RE:FindFirstChild("1RPNam1eTex1t")
        if TextRemote then
            TextRemote:FireServer("RolePlayName", "")
            TextRemote:FireServer("RolePlayBio", "")
        end
        local ColorRemote = RE:FindFirstChild("1RPNam1eColo1r")
        if ColorRemote then
            ColorRemote:FireServer("PickingRPNameColor", Color3.fromRGB(255, 255, 255))
            ColorRemote:FireServer("PickingRPBioColor", Color3.fromRGB(255, 255, 255))
        end
    end)
end

-- ========== دالة تشغيل الألقاب + البيو الافتراضية ==========
local function startRainbowTitle(titleText, bioText)
    if isRainbowActive then
        isRainbowActive = false
        task.wait(0.2)
    end
    clearTitleAndBio()
    task.wait(0.1)
    isRainbowActive = true
    pcall(function()
        local RE = game:GetService("ReplicatedStorage"):WaitForChild("RE")
        local TextRemote = RE:FindFirstChild("1RPNam1eTex1t")
        if TextRemote then
            TextRemote:FireServer("RolePlayName", titleText)
            TextRemote:FireServer("RolePlayBio", bioText)
        end
    end)
    task.spawn(LPH_NO_VIRTUALIZE(function()
        local ColorRemote = game:GetService("ReplicatedStorage"):WaitForChild("RE"):FindFirstChild("1RPNam1eColo1r")
        if not ColorRemote then return end
        local hue = 0
        while isRainbowActive do
            local color = Color3.fromHSV(hue, 1, 1)
            pcall(LPH_NO_VIRTUALIZE(function()
                ColorRemote:FireServer("PickingRPNameColor", color)
                ColorRemote:FireServer("PickingRPBioColor", color)
            end))
            hue = (hue + 0.04) % 1
            task.wait(speedIntervals[currentSpeedLevel] or 0.1)
        end
    end))
end

-- ========== تشغيل ريموتات الماب والتفعيل التلقائي ==========
task.spawn(function()
    pcall(function()
        local RE = game:GetService("ReplicatedStorage"):WaitForChild("RE")
        local Telemetry = game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("TelemetryClientInteraction")
        Telemetry:FireServer("uiInteraction", {["buttonName"] = "Popout", ["inVehicle"] = false})
        task.wait(0.1)
        Telemetry:FireServer("uiInteraction", {["buttonName"] = "NameHudButton", ["inVehicle"] = false})
        local FlyingRemote = RE:WaitForChild("1Flyin1g")
        if FlyingRemote then
            FlyingRemote:FireServer("CheckForServerOwner")
            FlyingRemote:FireServer("PCollisionPatch")
        end
    end)
    task.wait(1.5)
    if isFirstRun then
        startRainbowTitle(defaultTitleText, defaultBioText)
        isFirstRun = false
    end
end)

-- تأثير ترحيبي عند التشغيل
task.spawn(function()
    wait(2)
    Rayfield:Notify({
        Title = " - منور/ه السكربت يا عسل " .. DisplayName .. " 🏴",
        Content = "تم تشغيل HDX HUB V3.5 بنجاح!",
        Duration = 5
    })
end)

local Window = Rayfield:CreateWindow({
   Name = "Alsaadi | Dark_Santos090 V3.5", 
   LoadingTitle = " ♾️ جاري تشغيل السكربت المطور.. ♾️  ", 
   LoadingSubtitle = "Alsaadi",
   ConfigurationSaving = {Enabled = true, FolderName = "HDXHubGreat", FileName = "MainConfigV3"},
   Theme = "BlackConsole", 
})

-- 1. تبويب السكربتات
local MainTab = Window:CreateTab("الــســكــربــتات", 4483362458)
MainTab:CreateSection("قــائــمــة الــســكــربــتات الفحوليه 🥺")
MainTab:CreateButton({Name = "ســكـــربــــت IY 🌪", Callback = function() loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Infinite-Yield-v64-90090"))() end})
MainTab:CreateButton({Name = "ســكـــربــــت بــروتــون 🍃", Callback = function() loadstring(game:HttpGet("https://rawscripts.net/raw/Brookhaven-RP-BRUTON-HUB-IS-BACK-236137"))()() end})
MainTab:CreateButton({Name = "🧨 R4D ســكـــربــــت", Callback = function() loadstring(game:HttpGet("https://rawscripts.net/raw/Brookhaven-RP-R4D-TROLL-NO-KEY-17625"))()() end})
MainTab:CreateButton({Name = "ســكـــربــــت ديــــمــوز ⚜️", Callback = function() loadstring(game:HttpGet("https://raw.githubusercontent.com/SuperTradei/Super/refs/heads/main/sfratolo23%20(1).txt"))() end})
MainTab:CreateButton({Name = "Alsaadi ســكـــربــــت الــطــيــران 〽️", Callback = function() loadstring(game:HttpGet("https://pastefy.app/qE6dnXi6/raw"))() end})
MainTab:CreateButton({Name = "ســكـــربــــت رقــصـات ✨", Callback = function() loadstring(game:HttpGet("https://raw.githubusercontent.com/7yd7/Hub/refs/heads/Branch/GUIS/Emotes.lua"))() end})
MainTab:CreateButton({Name = "ســكـــربــــت JG 🪯", Callback = function() loadstring(game:HttpGet("https://raw.githubusercontent.com/joygril/Brookhaven-RP-JG-Hub/refs/heads/main/Jeon-The-Best.txt"))() end})
MainTab:CreateButton({Name = "ســكـــربــــت VR7 ❗", Callback = function() loadstring(game:HttpGet("https://raw.githubusercontent.com/Hm5011/hussain/refs/heads/main/Mercy%20Script"))() end})
MainTab:CreateButton({Name = "ســكـــربــــت اغــانـي 📻", Callback = function() loadstring(game:HttpGet("https://gist.githubusercontent.com/forbid-unknown/68e5a6253095fd2ca38fb6e4edf802cc/raw/f6afc762405b2dc02368288db60ab4808ef73a1c/fm.lua"))() end})
MainTab:CreateButton({Name = "ســكـــربــــت FH7 ❌", Callback = function() loadstring(game:HttpGet("https://raw.githubusercontent.com/DevFHD1/FH7-Script/refs/heads/main/Q"))() end})
MainTab:CreateButton({Name = "ســكـــربــــت صــمــلات ♏", Callback = function() loadstring(game:HttpGet("https://raw.githubusercontent.com/mourad1-hub/N9atSMLAT/refs/heads/main/Protected_9506248386709347.lua.txt"))() end})
MainTab:CreateButton({Name = "ســكـــربــــت الــفــا 🧟‍♂️", Callback = function() loadstring(game:HttpGet("https://raw.githubusercontent.com/ALFA5675/ALFA/refs/heads/main/ALFA_IRAQ.lua.txt"))() end})

-- 2. تبويب الإحداثيات والمواقع
local TeleportTab = Window:CreateTab("📍 الإحداثيات والمواقع", 4483362458)
TeleportTab:CreateSection("قائمة مواقع الصور المستخرجة")
TeleportTab:CreateButton({Name = "موقع 1", Callback = function() teleportTo(-694.14, 260.01, 794.86) end})
TeleportTab:CreateButton({Name = "موقع 2", Callback = function() teleportTo(-34.52, 243.38, 14.15) end})
TeleportTab:CreateButton({Name = "موقع 3", Callback = function() teleportTo(696.74, 3.00, -371.92) end})
TeleportTab:CreateButton({Name = "موقع 4", Callback = function() teleportTo(-1291.18, 76.00, -1339.56) end})
TeleportTab:CreateButton({Name = "موقع 5", Callback = function() teleportTo(327.47, 93.10, 100.99) end})
TeleportTab:CreateButton({Name = "موقع 6", Callback = function() teleportTo(59.03, 3.02, -522.41) end})
TeleportTab:CreateButton({Name = "موقع 7", Callback = function() teleportTo(658.51, -111.84, 503.66) end})

-- 3. تبويب متحكم الألقاب & RGB
local TitleTab = Window:CreateTab("🌈 متحكم الألقاب & RGB", 4483362458)
TitleTab:CreateSection("👑 لوحة التحكم بالسرعة")
local SpeedDisplay = TitleTab:CreateLabel("⚡ السرعة الحالية: " .. currentSpeedLevel)
TitleTab:CreateButton({
    Name = "🚀 زيادة السرعة (تسريع)",
    Callback = function()
        if currentSpeedLevel < 10 then
            currentSpeedLevel = currentSpeedLevel + 1
            SpeedDisplay:Set("⚡ السرعة الحالية: " .. (currentSpeedLevel == 10 and "MAX 🔥" or currentSpeedLevel))
        end
    end
})
TitleTab:CreateButton({
    Name = "🐢 تقليل السرعة (تبطيء)",
    Callback = function()
        if currentSpeedLevel > 1 then
            currentSpeedLevel = currentSpeedLevel - 1
            SpeedDisplay:Set("⚡ السرعة الحالية: " .. currentSpeedLevel)
        end
    end
})

-- 4. تبويب كودات الأغاني (مع القائمة الموسعة وزر نسخ الكل)
local CodesTab = Window:CreateTab("🎵 كــودات الأغــانــي", 4483362458)
CodesTab:CreateSection("📋 قائمة كودات الأغاني")
local songCodes = {
    {name = "🎵 لو حكو عنك اموت", code = "136775741963432"},
    {name = "🎵 يشتغل فقط بسكربت R4D - حرام عليك", code = "0x0000000000000006B8E78719165"},
    {name = "🎵 من غبت عني", code = "93297302504653"},
    {name = "🎵 ليبي علمني", code = "107273226047360"},
    {name = "🎵 لو تبوس ادينا", code = "80039364766636"},
    {name = "🎵 ماريدك بعد تهواني", code = "135911328646170"},
    {name = "🎵 حنيتك", code = "73632319736202"},
    {name = "🎵 مصري مُره", code = "132378395114388"},
    {name = "🎵 خليجي انا شكاي", code = "80469096504953"},
    {name = "🎵 جفاني", code = "126189830749452"},
    {name = "🎵 ياخذني طيفك", code = "106271890575602"},
    {name = "🎵 مصري بقيالو", code = "82972945719527"},
    {name = "🎵 مهموم", code = "139296082581664"},
    {name = "🎵 اجنبي", code = "99472699182002"}
}

for _, song in ipairs(songCodes) do
    CodesTab:CreateButton({
        Name = song.name,
        Callback = function()
            setclipboard(song.code)
            Rayfield:Notify({Title = "✅ تم النسخ", Content = "تم نسخ الكود: " .. song.code, Duration = 2})
        end
    })
end

CodesTab:CreateSection("📦 نسخ الكل")
CodesTab:CreateButton({
    Name = "📋 نسخ جميع الكودات دفعة واحدة",
    Callback = function()
        local allCodes = ""
        for _, song in ipairs(songCodes) do
            allCodes = allCodes .. song.code .. "\n"
        end
        setclipboard(allCodes)
        Rayfield:Notify({Title = "✅ تم النسخ", Content = "تم نسخ جميع الكودات (" .. #songCodes .. " كود)", Duration = 3})
    end
})

-- 5. تبويب الحماية ULTRA PRO MAX
local ProtectionTab = Window:CreateTab("🛡️ الــحــمــايــة PRO MAX", 4483362458)
ProtectionTab:CreateSection("🔥 حماية شاملة ومتطورة")

ProtectionTab:CreateToggle({
    Name = "🛡️ تفعيل الحماية الشاملة (All-in-One)",
    CurrentValue = false,
    Callback = function(state)
        UltraEnabled = state
        AntiKickEnabled = state
        AntiFreezeEnabled = state
        AntiCrashEnabled = state
        AntiLagEnabled = state
        setupUltraProtection()
        Rayfield:Notify({
            Title = state and "🛡️ تم التفعيل الكامل" or "🔓 تم الإيقاف",
            Content = state and "تم تفعيل جميع أنظمة الحماية المتطورة" or "تم إيقاف جميع أنظمة الحماية",
            Duration = 4
        })
    end
})

ProtectionTab:CreateSection("⚙️ أنظمة الحماية الفردية")
ProtectionTab:CreateToggle({
    Name = "🚫 منع الطرد (Anti-Kick)",
    CurrentValue = false,
    Callback = function(state)
        AntiKickEnabled = state
        setupAntiKick()
        Rayfield:Notify({Title = state and "✅ تم التفعيل" or "❌ تم الإيقاف", Content = "نظام منع الطرد", Duration = 3})
    end
})

ProtectionTab:CreateToggle({
    Name = "🧊 منع التجميد (Anti-Freeze)",
    CurrentValue = false,
    Callback = function(state)
        AntiFreezeEnabled = state
        setupAntiFreeze()
        Rayfield:Notify({Title = state and "✅ تم التفعيل" or "❌ تم الإيقاف", Content = "نظام منع التجميد", Duration = 3})
    end
})

ProtectionTab:CreateToggle({
    Name = "💥 منع الكراش (Anti-Crash)",
    CurrentValue = false,
    Callback = function(state)
        AntiCrashEnabled = state
        setupAntiCrash()
        Rayfield:Notify({Title = state and "✅ تم التفعيل" or "❌ تم الإيقاف", Content = "نظام منع الكراش", Duration = 3})
    end
})

ProtectionTab:CreateToggle({
    Name = "⚡ منع اللاق (Anti-Lag)",
    CurrentValue = false,
    Callback = function(state)
        AntiLagEnabled = state
        setupAntiLag()
        Rayfield:Notify({Title = state and "✅ تم التفعيل" or "❌ تم الإيقاف", Content = "نظام منع اللاق وتحسين الأداء", Duration = 3})
    end
})

-- 6. تبويب اسم المستخدم ومعلومات الحساب والمطور
local UserTab = Window:CreateTab("👤 اسم المستخدم", 4483362458)
UserTab:CreateSection("معلومات حسابك الحالي")
UserTab:CreateLabel("👤 اسم العرض: " .. DisplayName)
UserTab:CreateLabel("📛 اسم المستخدم: " .. Name)
UserTab:CreateLabel("🆔 الآدي (ID): " .. UserID)
UserTab:CreateLabel("⚡ الإكسبلورتر: " .. Executor)

UserTab:CreateSection("👑 حقوق المطور الرسمي")
UserTab:CreateLabel("📌 Alsaadi مطور السكربت ")
UserTab:CreateLabel("🏴 براند السكربت: HDX HUB V3.5")
UserTab:CreateButton({
    Name = "📋 نسخ اسم حساب روبلوكس: Dark_Santos090", 
    Callback = function() 
        setclipboard("Dark_Santos090") 
        Rayfield:Notify({Title = "✅ تم النسخ", Content = "Dark_Santos090", Duration = 3}) 
    end
})
UserTab:CreateButton({
    Name = "🎵 نسخ تيك توك المطور الجديد", 
    Callback = function() 
        setclipboard("https://www.tiktok.com/@crszyboombrepzyxx?_r=1&_t=ZS-99vEd480V3U") 
        Rayfield:Notify({Title = "✅ تم النسخ", Content = "رابط تيك توك: crszyboombrepzyxx", Duration = 3}) 
    end
})
