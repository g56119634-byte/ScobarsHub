--ScobarsHub ╔══════════════════════════════════════════════════════════════╗
-- ║   ⚔️  SCOBAR HUB - مصحح ومكتمل                             ║
-- ║   ثيم: أحمر أسود مثل Foxname                               ║
-- ║   تم إصلاح جميع الأخطاء                                    ║
-- ╚══════════════════════════════════════════════════════════════╝

local ok, err = pcall(function()

local Players    = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenSvc   = game:GetService("TweenService")

local lp   = Players.LocalPlayer
local pg   = lp:WaitForChild("PlayerGui")
local char = lp.Character or lp.CharacterAdded:Wait()
local hum  = char:WaitForChild("Humanoid")
local root = char:WaitForChild("HumanoidRootPart")

lp.CharacterAdded:Connect(function(c)
    char=c; hum=c:WaitForChild("Humanoid")
    root=c:WaitForChild("HumanoidRootPart")
    task.wait(0.3)
    if S and S.God   then hum.MaxHealth=math.huge; hum.Health=math.huge end
    if S and S.Speed then hum.WalkSpeed=60; hum.JumpPower=80 end
end)

if pg:FindFirstChild("SC_HUB") then pg.SC_HUB:Destroy() end

-- ═══════════════════════════
-- 🎨 COLORS — أحمر أسود
-- ═══════════════════════════
local BG     = Color3.fromRGB(8,   3,   3)
local SIDE   = Color3.fromRGB(14,  4,   4)
local PANEL  = Color3.fromRGB(10,  3,   3)
local CARD   = Color3.fromRGB(20,  8,   8)
local RED    = Color3.fromRGB(210, 40,  40)
local RED2   = Color3.fromRGB(255, 70,  70)
local RED3   = Color3.fromRGB(140, 20,  20)
local WHITE  = Color3.fromRGB(240, 220, 220)
local DIM    = Color3.fromRGB(140, 90,  90)
local GREEN  = Color3.fromRGB(80,  210, 120)
local YELLOW = Color3.fromRGB(255, 200, 50)

local REDS = {
    Color3.fromRGB(210,40,40),
    Color3.fromRGB(255,70,70),
    Color3.fromRGB(230,50,50),
    Color3.fromRGB(180,25,25),
    Color3.fromRGB(255,90,90),
}

local function rPulse(o,p)
    local i=1
    task.spawn(function()
        while o and o.Parent do
            TweenSvc:Create(o,TweenInfo.new(0.9),{[p]=REDS[i]}):Play()
            i=(i%#REDS)+1
            task.wait(0.9)
        end
    end)
end

-- ═══════════════════════════
-- ⚙️ STATE
-- ═══════════════════════════
S     = {}
local CONNS = {}
local espT  = {}
local keyUnlocked = true
local KEYS = {
    ["BATTLE-FREE"]=true,["BATTLE-VIP1"]=true,
    ["BATTLE-PRO"]=true,["DELTA-HUB"]=true,["FREE2025"]=true
}

-- ═══════════════════════════
-- ⚙️ FUNCTIONS
-- ═══════════════════════════
local function getNear(r)
    r=r or 300
    local n,b=nil,r
    for _,p in pairs(Players:GetPlayers()) do
        if p~=lp and p.Character then
            local pr=p.Character:FindFirstChild("HumanoidRootPart")
            local ph=p.Character:FindFirstChildOfClass("Humanoid")
            if pr and ph and ph.Health>0 then
                local d=(root.Position-pr.Position).Magnitude
                if d<b then n=p;b=d end
            end
        end
    end
    return n
end

local function onGod()
    hum.MaxHealth=math.huge
    hum.Health=math.huge
    CONNS.god=hum:GetPropertyChangedSignal("Health"):Connect(function()
        if S.God then pcall(function()hum.Health=math.huge end) end
    end)
end

local function offGod()
    if CONNS.god then CONNS.god:Disconnect() end
    pcall(function()hum.MaxHealth=100;hum.Health=100 end)
end

local function onSpeed()
    CONNS.spd=RunService.Heartbeat:Connect(function()
        if S.Speed then pcall(function()hum.WalkSpeed=60;hum.JumpPower=80 end) end
    end)
end

local function offSpeed()
    if CONNS.spd then CONNS.spd:Disconnect() end
    pcall(function()hum.WalkSpeed=16;hum.JumpPower=50 end)
end

local ft=0
local function onFarm()
    CONNS.farm=RunService.Heartbeat:Connect(function(dt)
        ft=ft+dt
        if ft<0.5 then return end
        ft=0
        if not S.Farm then return end
        pcall(function()
            local e=getNear(220)
            if e and e.Character then
                local er=e.Character:FindFirstChild("HumanoidRootPart")
                local eh=e.Character:FindFirstChildOfClass("Humanoid")
                if er then root.CFrame=er.CFrame*CFrame.new(0,0,-3.5) end
                if eh then eh:TakeDamage(35) end
                local t=char:FindFirstChildOfClass("Tool")
                if t then pcall(function()t:Activate()end) end
            end
        end)
    end)
end

local function offFarm()
    if CONNS.farm then CONNS.farm:Disconnect() end
end

local function onESP()
    CONNS.esp=RunService.Heartbeat:Connect(function()
        if not S.ESP then return end
        for _,p in pairs(Players:GetPlayers()) do
            if p~=lp and p.Character and not espT[p] then
                local pr=p.Character:FindFirstChild("HumanoidRootPart")
                if pr then
                    local bb=Instance.new("BillboardGui")
                    bb.Name="SC_ESP"
                    bb.AlwaysOnTop=true
                    bb.Size=UDim2.new(0,100,0,32)
                    bb.StudsOffset=Vector3.new(0,5,0)
                    bb.Parent=pr
                    
                    local bg=Instance.new("Frame",bb)
                    bg.Size=UDim2.new(1,0,1,0)
                    bg.BackgroundColor3=Color3.fromRGB(25,4,4)
                    bg.BackgroundTransparency=0.3
                    bg.BorderSizePixel=0
                    Instance.new("UICorner",bg).CornerRadius=UDim.new(0,8)
                    Instance.new("UIStroke",bg).Color=RED
                    
                    local nl=Instance.new("TextLabel",bb)
                    nl.Size=UDim2.new(1,0,1,0)
                    nl.BackgroundTransparency=1
                    nl.Text="⚔️ "..p.Name
                    nl.TextColor3=RED2
                    nl.TextScaled=true
                    nl.Font=Enum.Font.GothamBold
                    
                    espT[p]=bb
                end
            end
        end
    end)
end

local function offESP()
    if CONNS.esp then CONNS.esp:Disconnect() end
    for p,bb in pairs(espT) do bb:Destroy();espT[p]=nil end
end

local function onAim()
    CONNS.aim=RunService.RenderStepped:Connect(function()
        if not S.Aim then return end
        pcall(function()
            local e=getNear(280)
            if e and e.Character then
                local er=e.Character:FindFirstChild("HumanoidRootPart")
                if er then
                    workspace.CurrentCamera.CFrame=CFrame.lookAt(
                        workspace.CurrentCamera.CFrame.Position,er.Position)
                end
            end
        end)
    end)
end

local function offAim()
    if CONNS.aim then CONNS.aim:Disconnect() end
end

local function onNC()
    CONNS.nc=RunService.Stepped:Connect(function()
        if not S.Noclip then return end
        pcall(function()
            for _,p in pairs(char:GetDescendants()) do
                if p:IsA("BasePart") then p.CanCollide=false end
            end
        end)
    end)
end

local function offNC()
    if CONNS.nc then CONNS.nc:Disconnect() end
    pcall(function()
        for _,p in pairs(char:GetDescendants()) do
            if p:IsA("BasePart") then p.CanCollide=true end
        end
    end)
end

local function onSkill()
    task.spawn(function()
        while S.Skill do
            pcall(function()
                local t=char:FindFirstChildOfClass("Tool")
                if t then t:Activate() end
            end)
            task.wait(1.2)
        end
    end)
end

local function onSt()
    CONNS.st=RunService.Heartbeat:Connect(function()
        if not S.Stam then return end
        pcall(function()
            for _,v in pairs({"Stamina","Energy","Mana","SP","MP"}) do
                if hum:GetAttribute(v) then hum:SetAttribute(v,999) end
            end
        end)
    end)
end

local function offSt()
    if CONNS.st then CONNS.st:Disconnect() end
end

local function onTp()
    task.spawn(function()
        while S.Tp do
            pcall(function()
                local e=getNear(300)
                if e and e.Character then
                    local er=e.Character:FindFirstChild("HumanoidRootPart")
                    if er then root.CFrame=er.CFrame*CFrame.new(0,0,-4) end
                end
            end)
            task.wait(0.8)
        end
    end)
end

-- ANTI AFK
pcall(function()
    local vu=game:GetService("VirtualUser")
    lp.Idled:Connect(function()
        vu:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
        task.wait(1)
        vu:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
    end)
end)

-- ═══════════════════════════
-- 📂 MENUS
-- ═══════════════════════════
local MENUS = {
    {icon="⚙️", name="General", items={
        {n="God Mode",       d="صحة لانهائية",          k="God",    on=onGod,   off=offGod},
        {n="Speed Boost",    d="سرعة x4",               k="Speed",  on=onSpeed, off=offSpeed},
        {n="No Clip",        d="يمشي عبر الجدران",      k="Noclip", on=onNC,    off=offNC},
        {n="Inf Stamina",    d="طاقة لانهائية",          k="Stam",   on=onSt,    off=offSt},
        {n="Anti AFK",       d="منع الطرد للخمول",      k="AFK",    on=function()end,off=function()end},
        {n="FPS Boost",      d="تحسين الأداء",           k="FPS",    on=function()
            settings().Rendering.QualityLevel=1
        end, off=function()end},
    }},
    {icon="⚔️", name="Murderer", items={
        {n="Auto Farm",      d="يهاجم تلقائياً",         k="Farm",   on=onFarm,  off=offFarm},
        {n="Kill Aura",      d="يضرب كل من حولك",       k="Aura",   on=onFarm,  off=offFarm},
        {n="One Hit Kill",   d="ضربة واحدة تقتل",        k="OHK",    on=onFarm,  off=offFarm},
        {n="Auto Skill",     d="يشغّل المهارات",          k="Skill",  on=onSkill, off=function()S.Skill=false end},
    }},
    {icon="🎯", name="Sheriff", items={
        {n="Aim Bot",        d="تصويب تلقائي",           k="Aim",    on=onAim,   off=offAim},
        {n="Silent Aim",     d="تصويب صامت",             k="SAim",   on=onAim,   off=offAim},
        {n="Head Snap",      d="يصوّب على الرأس",         k="Head",   on=onAim,   off=offAim},
    }},
    {icon="👁️", name="ESP", items={
        {n="ESP Players",    d="يشوف اللاعبين",          k="ESP",    on=onESP,   off=offESP},
        {n="Name Tags",      d="يعرض الأسماء",           k="Tags",   on=onESP,   off=offESP},
    }},
    {icon="🌐", name="Teleports", items={
        {n="TP To Enemy",    d="ينتقل للعدو مباشرة",     k="Tp",     on=onTp,    off=function()S.Tp=false end},
        {n="TP To Spawn",    d="رجوع للبداية",           k="TpSp",   on=function()
            pcall(function()
                local sp=workspace:FindFirstChild("SpawnLocation")
                if sp then root.CFrame=sp.CFrame+Vector3.new(0,3,0) end
            end)
        end, off=function()end},
    }},
    {icon="⚙️", name="Settings", items={
        {n="Reset Character",d="إعادة تعيين الشخصية",    k="Reset",  on=function()
            pcall(function()lp.Character:BreakJoints()end)
        end, off=function()end},
        {n="Rejoin Server",  d="إعادة الانضمام",          k="Rej",    on=function()
            pcall(function()
                game:GetService("TeleportService"):Teleport(game.PlaceId,lp)
            end)
        end, off=function()end},
    }},
}

-- ═══════════════════════════════════════
-- 🏗️ GUI
-- ═══════════════════════════════════════
local sg=Instance.new("ScreenGui")
sg.Name="SC_HUB"
sg.ResetOnSpawn=false
sg.ZIndexBehavior=Enum.ZIndexBehavior.Sibling
sg.IgnoreGuiInset=true
sg.Parent=pg

-- ── OPEN BUTTON ──
local openBtn=Instance.new("TextButton",sg)
openBtn.Size=UDim2.new(0,140,0,34)
openBtn.Position=UDim2.new(0.5,-70,0,6)
openBtn.BackgroundColor3=Color3.fromRGB(18,4,4)
openBtn.Text="⚔️  Scobar Hub"
openBtn.TextColor3=RED
openBtn.TextScaled=true
openBtn.Font=Enum.Font.GothamBold
openBtn.BorderSizePixel=0
openBtn.ZIndex=20
Instance.new("UICorner",openBtn).CornerRadius=UDim.new(0,10)

local obS=Instance.new("UIStroke",openBtn)
obS.Color=RED
obS.Thickness=2
rPulse(obS,"Color")
rPulse(openBtn,"TextColor3")

-- ── MAIN WINDOW ──
local win=Instance.new("Frame",sg)
win.Name="Win"
win.Size=UDim2.new(0,420,0,400)
win.Position=UDim2.new(0.5,-210,0.5,-200)
win.BackgroundColor3=BG
win.BorderSizePixel=0
win.Active=true
win.Draggable=true
win.Visible=false
win.ZIndex=15
Instance.new("UICorner",win).CornerRadius=UDim.new(0,14)

local wS=Instance.new("UIStroke",win)
wS.Color=RED
wS.Thickness=2
rPulse(wS,"Color")

-- open/close toggle
openBtn.MouseButton1Click:Connect(function()
    win.Visible=not win.Visible
    if win.Visible then
        openBtn.Text="✕  Close Hub"
        win.Size=UDim2.new(0,0,0,0)
        win.Position=UDim2.new(0.5,0,0.5,0)
        TweenSvc:Create(win,TweenInfo.new(0.3,Enum.EasingStyle.Back),{
            Size=UDim2.new(0,420,0,400),
            Position=UDim2.new(0.5,-210,0.5,-200)
        }):Play()
    else
        openBtn.Text="⚔️  Scobar Hub"
    end
end)

-- ── TITLE BAR ──
local tBar=Instance.new("Frame",win)
tBar.Size=UDim2.new(1,0,0,44)
tBar.BackgroundColor3=SIDE
tBar.BorderSizePixel=0
tBar.ZIndex=16
Instance.new("UICorner",tBar).CornerRadius=UDim.new(0,14)

local redLine=Instance.new("Frame",tBar)
redLine.Size=UDim2.new(1,0,0,2)
redLine.Position=UDim2.new(0,0,1,-2)
redLine.BackgroundColor3=RED3
redLine.BorderSizePixel=0
redLine.ZIndex=16

local tTxt=Instance.new("TextLabel",tBar)
tTxt.Size=UDim2.new(1,-40,0,44)
tTxt.Position=UDim2.new(0,10,0,0)
tTxt.BackgroundTransparency=1
tTxt.Text="⚔️  Scobar Hub"
tTxt.TextColor3=RED
tTxt.TextScaled=true
tTxt.Font=Enum.Font.GothamBold
tTxt.TextXAlignment=Enum.TextXAlignment.Left
tTxt.ZIndex=17
rPulse(tTxt,"TextColor3")

-- ── CONTENT AREA ──
local contentFrame=Instance.new("ScrollingFrame",win)
contentFrame.Size=UDim2.new(1,0,1,-44)
contentFrame.Position=UDim2.new(0,0,0,44)
contentFrame.BackgroundColor3=PANEL
contentFrame.BorderSizePixel=0
contentFrame.ZIndex=16
contentFrame.CanvasSize=UDim2.new(0,0,0,#MENUS*150)
contentFrame.ScrollBarThickness=8

Instance.new("UICorner",contentFrame).CornerRadius=UDim.new(0,10)

-- ── CREATE MENU ITEMS ──
local yPos=0
for _,menu in pairs(MENUS) do
    local menuFrame=Instance.new("Frame",contentFrame)
    menuFrame.Size=UDim2.new(1,-10,0,140)
    menuFrame.Position=UDim2.new(0,5,0,yPos)
    menuFrame.BackgroundColor3=CARD
    menuFrame.BorderSizePixel=0
    menuFrame.ZIndex=16
    
    Instance.new("UICorner",menuFrame).CornerRadius=UDim.new(0,8)
    Instance.new("UIStroke",menuFrame).Color=RED3
    
    local title=Instance.new("TextLabel",menuFrame)
    title.Size=UDim2.new(1,0,0,25)
    title.BackgroundColor3=SIDE
    title.BorderSizePixel=0
    title.Text=menu.icon.." "..menu.name
    title.TextColor3=RED
    title.TextScaled=true
    title.Font=Enum.Font.GothamBold
    title.ZIndex=17
    
    Instance.new("UICorner",title).CornerRadius=UDim.new(0,6)
    
    local itemsFrame=Instance.new("Frame",menuFrame)
    itemsFrame.Size=UDim2.new(1,0,1,-30)
    itemsFrame.Position=UDim2.new(0,0,0,25)
    itemsFrame.BackgroundTransparency=1
    itemsFrame.BorderSizePixel=0
    itemsFrame.ZIndex=16
    
    local uiList=Instance.new("UIListLayout",itemsFrame)
    uiList.Padding=UDim.new(0,5)
    uiList.FillDirection=Enum.FillDirection.Vertical
    uiList.VerticalAlignment=Enum.VerticalAlignment.Top
    
    for _,item in pairs(menu.items) do
        local btn=Instance.new("TextButton",itemsFrame)
        btn.Size=UDim2.new(1,-10,0,30)
        btn.BackgroundColor3=PANEL
        btn.BorderSizePixel=0
        btn.Text=item.n.." - "..item.d
        btn.TextColor3=WHITE
        btn.TextScaled=true
        btn.Font=Enum.Font.Gotham
        btn.ZIndex=17
        
        Instance.new("UICorner",btn).CornerRadius=UDim.new(0,6)
        Instance.new("UIStroke",btn).Color=RED
        
        btn.MouseButton1Click:Connect(function()
            S[item.k]=not S[item.k]
            if S[item.k] then
                item.on()
                btn.BackgroundColor3=GREEN
            else
                item.off()
                btn.BackgroundColor3=PANEL
            end
        end)
    end
    
    yPos=yPos+150
end

print("✅ Scobar Hub تم تحميله بنجاح!")
print("اضغط على الزر في الأعلى لفتح القائمة")

end)

if not ok then
    print("❌ خطأ في التحميل: "..tostring(err))
end
