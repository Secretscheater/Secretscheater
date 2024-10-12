- 👋 Hi, I’m @Secretscheater
- 👀 I’m interested in exploit
- 🌱 I’m currently learning making exploit
- 💞️ I’m looking to collaborate on nothing
- 📫 How to reach me nothing
- 😄 Pronouns: he
- ⚡ Fun fact: nothing

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")

-- Function to create and update the highlight for a player
local function highlightPlayer(player)
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        -- Create a BillboardGui to display the player's name
        local billboard = Instance.new("BillboardGui")
        billboard.Size = UDim2.new(0, 100, 0, 50)  -- Adjust size as needed
        billboard.StudsOffset = Vector3.new(0, 3, 0)  -- Position above the player
        billboard.Parent = player.Character.HumanoidRootPart

        -- Create a TextLabel to show the player's name
        local textLabel = Instance.new("TextLabel")
        textLabel.Size = UDim2.new(1, 0, 1, 0)
        textLabel.BackgroundTransparency = 1
        textLabel.Text = player.Name
        textLabel.TextColor3 = Color3.new(1, 1, 1)  -- White text color
        textLabel.TextScaled = true
        textLabel.Parent = billboard

        -- Create a highlight effect
        local highlight = Instance.new("Highlight")
        highlight.Adornee = player.Character
        highlight.FillColor = Color3.new(1, 1, 0)  -- Yellow highlight
        highlight.Parent = player.Character

        -- Update the visibility of the BillboardGui based on visibility
        local function updateVisibility()
            if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                local playerPosition = player.Character.HumanoidRootPart.Position
                local localPlayerPosition = LocalPlayer.Character and LocalPlayer.Character.HumanoidRootPart.Position

                if localPlayerPosition then
                    -- Raycast to check for obstructions
                    local ray = Ray.new(localPlayerPosition, (playerPosition - localPlayerPosition).unit * (localPlayerPosition - playerPosition).magnitude)
                    local hitPart = workspace:FindPartOnRay(ray, LocalPlayer.Character)

                    -- Show or hide the BillboardGui based on visibility
                    billboard.Enabled = hitPart == nil or hitPart:IsDescendantOf(workspace)
                end
            end
        end

        -- Connect the update function to RenderStepped
        RunService.RenderStepped:Connect(updateVisibility)
    end
end

-- Function to remove highlight when the player leaves
local function onPlayerRemoving(player)
    if player.Character then
        local highlight = player.Character:FindFirstChildOfClass("Highlight")
        if highlight then
            highlight:Destroy()
        end
    end
end

-- Connect the highlight function to each player
for _, player in ipairs(Players:GetPlayers()) do
    highlightPlayer(player)
end

-- Listen for new players joining
Players.PlayerAdded:Connect(highlightPlayer)

-- Listen for players leaving
Players.PlayerRemoving:Connect(onPlayerRemoving)
