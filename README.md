local Lighting = game:GetService("Lighting")
local Terrain = workspace.Terrain

local REMOVER_PARTICULAS = true

pcall(function()
    settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
end)
Lighting.GlobalShadows = false
Lighting.FogEnd = 9e9
Lighting.Brightness = 1

for _, v in ipairs(Lighting:GetChildren()) do
    if v:IsA("PostEffect") or v:IsA("Atmosphere") then
        pcall(function()
            if v:IsA("Atmosphere") then v.Density = 0 else v.Enabled = false end
        end)
    end
end

pcall(function()
    Terrain.WaterWaveSize = 0
    Terrain.WaterWaveSpeed = 0
    Terrain.WaterReflectance = 0
    Terrain.WaterTransparency = 1
end)

local function otimizar(obj)
    pcall(function()
        if obj:IsA("BasePart") then
            obj.Material = Enum.Material.SmoothPlastic
            obj.Reflectance = 0
            obj.CastShadow = false
        elseif obj:IsA("Decal") or obj:IsA("Texture") then
            obj.Transparency = 1
        elseif REMOVER_PARTICULAS and (obj:IsA("ParticleEmitter") or obj:IsA("Trail")
            or obj:IsA("Smoke") or obj:IsA("Fire") or obj:IsA("Sparkles")) then
            obj.Enabled = false
        end
    end)
end

for _, obj in ipairs(workspace:GetDescendants()) do
    otimizar(obj)
end

workspace.DescendantAdded:Connect(otimizar)
