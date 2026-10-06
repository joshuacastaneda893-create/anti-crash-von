-- made by discord.gg/dawg 
-- sabcom owners are pussy
local P = game:GetService("Players")
local J = { AnimationConstraint = true, Motor6D = true }

while not P.LocalPlayer do task.wait() end
local lp = P.LocalPlayer

local function kind(d)
    local j = J[d.ClassName]
    if (j or d:IsA("BasePart")) and not (d:FindFirstAncestorWhichIsA("Tool") or d:FindFirstAncestorWhichIsA("Accessory")) then
        return j and 1 or 2
    end
end

local function watch(p, c, t)
    local n, lost, q, cs = { 0, 0 }, { 0, 0 }, false, {}
    for _, d in c:GetDescendants() do
        local k = kind(d)
        if k then n[k] += 1 end
    end
    local function bare() return n[1] <= 0 and n[2] >= 5 end
    local function remove()
        for _, x in cs do x:Disconnect() end
        if c.Parent then c:Destroy() print("nice try " .. p.Name) end
    end
    cs[1] = c.DescendantRemoving:Connect(function(d)
        local k = kind(d)
        if not k then return end
        n[k] -= 1
        lost[k] += 1
        if q then return end
        q = true
        task.defer(function()
            q = false
            if lost[1] > 0 and lost[2] == 0 then remove() end
            lost[1], lost[2] = 0, 0
        end)
    end)
    cs[2] = c.DescendantAdded:Connect(function(d)
        if d:IsA("Tool") then
            if bare() then task.defer(remove) end
        else
            local k = kind(d)
            if k then n[k] += 1 end
        end
    end)
    task.delay(t, function()
        if c.Parent and bare() then remove() end
    end)
end

local function track(p)
    if p == lp then return end
    if p.Character then watch(p, p.Character, 0) end
    p.CharacterAdded:Connect(function(c) watch(p, c, 1.5) end)
end

for _, p in P:GetPlayers() do track(p) end
P.PlayerAdded:Connect(track)

local g = Instance.new("ScreenGui")
g.IgnoreGuiInset, g.DisplayOrder = true, 2147483647
local l = Instance.new("TextLabel")
l.AnchorPoint, l.Position, l.Size, l.AutomaticSize, l.ZIndex = Vector2.new(0.5, 1), UDim2.new(0.25, 0, 1, -2), UDim2.new(), Enum.AutomaticSize.XY, 2147483647
l.BackgroundTransparency, l.Font, l.TextSize, l.TextColor3, l.Text = 1, Enum.Font.Gotham, 22, Color3.fromRGB(180, 0, 255), "anticrash on - discord.gg/dawg"
Instance.new("UIStroke", l).Thickness = 2
l.Parent = g
g.Parent = gethui and gethui() or game:GetService("CoreGui")

