local Players = game:GetService("Players")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
 
local playerGui = player:WaitForChild("PlayerGui")
local hotbar = playerGui:FindFirstChild("Hotbar")
local backpack = hotbar:FindFirstChild("Backpack")
local hotbarFrame = backpack:FindFirstChild("Hotbar")
 
local buttonData = {
    {name = "1", text = "Draining Barrage"},
    {name = "2", text = "Shadow Drag"},
    {name = "3", text = "Slam Punch"},
    {name = "4", text = "Rightfull Counter"}, -- Basemovesets renamer
}
 
for _, data in pairs(buttonData) do
    local baseButton = hotbarFrame:FindFirstChild(data.name).Base
    local ToolName = baseButton.ToolName
    ToolName.Text = data.text
end
 
local function waitForGui()
    while true do
        local screenGui = playerGui:FindFirstChild("ScreenGui")
        if screenGui then
            local magicHealthFrame = screenGui:FindFirstChild("MagicHealth")
            if magicHealthFrame then
                local textLabel = magicHealthFrame:FindFirstChild("TextLabel")
                if textLabel then
                    textLabel.Text = "Chaos Energy OverDrive!" -- Ult Renamer
                    return
                end
            end
        end
        wait(1)
    end
end
 
waitForGui()
 
local replacementAnimations = {
    ["10468665991"] = {id = "rbxassetid://12502664044", speed = 1.9},   -- In Order
    ["10466974800"] = {id = "rbxassetid://13560306510", speed = 1.9},  
    ["10471336737"] = {id = "rbxassetid://13501296372", speed = 1.9}, 
    ["12510170988"] = {id = "rbxassetid://14004235777", speed = 1.9},  -- Basemovesets
 
    ["10469493270"] = {id = "rbxassetid://17889458563", speed = 0.9},  -- In Order
    ["10469630950"] = {id = "rbxassetid://17889461810", speed = 0.9}, 
    ["10469639222"] = {id = "rbxassetid://17889471098", speed = 0.9}, -- M1s
    ["10469643643"] = {id = "rbxassetid://16944345619", speed = 0.9}, 
 
    ["11343318134"] = {id = "rbxassetid://13376869471", speed = 0.6},  -- In Order
    ["11365563255"] = {id = "rbxassetid://12618292188", speed = 0.8},  
    ["12983333733"] = {id = "rbxassetid://16062712948", speed = 0.8},  -- Ults
    ["13927612951"] = {id = "rbxassetid://", speed = 1.0}, 
    ["12447707844"] = {id = "rbxassetid://16734584478", speed = 1.7},  -- Ultimate Activation Animation
 
    ["15955393872"] = {id = "rbxassetid://", speed = 1.0},  -- Wall Combo 
    ["100558589307006"] = {id = "rbxassetid://", speed = 1.0},  -- Uppercut
    ["10470104242"] = {id = "rbxassetid://18435383478", speed = 1.9},  -- Downslam 
    ["10479335397"] = {id = "rbxassetid://13294790250", speed = 1.9},  -- FDash
    ["10480793962"] = {id = "rbxassetid://", speed = 1.0},  -- RightDash
    ["10480796021"] = {id = "rbxassetid://", speed = 1.0},  -- LeftDash
    ["10470389827"] = {id = "rbxassetid://", speed = 1.0},  -- Block 
 
 
 
}
 
local function onAnimationPlayed(animationTrack)
    local animationId = animationTrack.Animation.AnimationId:match("%d+")
    local replacement = replacementAnimations[animationId]
 
    if replacement then
        animationTrack:Stop()
        local newAnimation = Instance.new("Animation")
        newAnimation.AnimationId = replacement.id
        local newTrack = humanoid:LoadAnimation(newAnimation)
        newTrack:Play()
        newTrack:AdjustSpeed(replacement.speed) -- Apply speed to the new animation
    end
 
    if animationId == "Your_Ult_Animation_ID" then -- Doesn't Work
        wait(2)
        local ultMoves = {
            {buttonIndex = "1", name = "Ult Move name 1"},
            {buttonIndex = "2", name = "Ult Move name 2"},
            {buttonIndex = "3", name = "Ult Move name 3"}, -- Doesn't Work
            {buttonIndex = "4", name = "Ult Move name 4"},
        }
        for _, move in ipairs(ultMoves) do
            local baseButton = hotbarFrame:FindFirstChild(move.buttonIndex).Base
            baseButton.ToolName.Text = move.name
        end
    end
end
 
humanoid.AnimationPlayed:Connect(onAnimationPlayed)
 
player.CharacterAdded:Connect(function(newCharacter)
    character = newCharacter
    humanoid = newCharacter:WaitForChild("Humanoid")
    humanoid.AnimationPlayed:Connect(onAnimationPlayed)
end)
 
 
 
 
local player = game.Players.LocalPlayer -- Define the player
local character = player.Character or player.CharacterAdded:Wait() -- Get the character
local humanoid = character:WaitForChild("Humanoid") -- Get the humanoid
 
local function checkAndSendMessage(animationId)
    local messages = {
        [""] = "Message here", -- In Order
        [""] = "Message here",
        [""] = "Message here", -- Basemovesets
        [""] = "Message here",
 
        [""] = "Message here", -- In Order
        [""] = "Message here",
        [""] = "Message here", -- M1s
        [""] = "Message here",
 
 
        [""] = "Message here", -- In Order
        [""] = "Message here",
        [""] = "Message here", -- Ults
        [""] = "Message here",
        ["12447707844"] = "the ultimate lifeform has awakened..", -- Ultimate Activation
 
 
        [""] = "Message here", -- WallCombo
        [""] = "Message here", -- Uppercut
        [""] = "Message here", -- Downslam
        [""] = "Message here", -- FrontDash
        [""] = "Message here", -- RDash   -- Make the Numbers blank if you dont want Msgs
        [""] = "Message here", -- LDash
        [""] = "Message here", -- Block
 
    } 
 
    if messages[animationId] then
        task.wait(1) -- Use task.wait for better performance
        local args = {
            [1] = messages[animationId],
            [2] = "All"
        }
        game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer(unpack(args))
    end
end
 
local function onAnimationPlayed(animationTrack)
    local animationId = animationTrack.Animation.AnimationId:match("%d+")
    checkAndSendMessage(animationId)
end
 
-- Connect the AnimationPlayed event for the current humanoid
humanoid.AnimationPlayed:Connect(onAnimationPlayed)
 
-- Reconnect to the new character's humanoid when the character respawns
player.CharacterAdded:Connect(function(newCharacter)
    character = newCharacter
    humanoid = newCharacter:WaitForChild("Humanoid")
    humanoid.AnimationPlayed:Connect(onAnimationPlayed)
end)
 
 
 
 
local OriginalName1 = "Death Counter"
local OriginalName2 = "Table Flip"
local OriginalName3 = "Serious Punch" -- Original Ultimates Movesets Name
local OriginalName4 = "Omni Directional Punch"
 
local UltMovesetName1 = "here"
local UltMovesetName2 = "here"
local UltMovesetName3 = "here" -- New Name
local UltMovesetName4 = "here"
 
-- don't change
local player = game.Players.LocalPlayer
local playerGui = player.PlayerGui
local character = player.Character or player.CharacterAdded:Wait() -- Get character
 
local toolNamesToReplace = {
    ["1"] = {original = OriginalName1, new = UltMovesetName1},
    ["2"] = {original = OriginalName2, new = UltMovesetName2},
    ["3"] = {original = OriginalName3, new = UltMovesetName3},
    ["4"] = {original = OriginalName4, new = UltMovesetName4}
}
 
local function checkAndReplaceToolName()
    while character.Humanoid.Health > 0 do
        local hotbar = playerGui:FindFirstChild("Hotbar")
        if hotbar then
            local backpack = hotbar:FindFirstChild("Backpack")
            if backpack then
                local hotbarFrame = backpack:FindFirstChild("Hotbar")
                if hotbarFrame then
                    for buttonName, toolData in pairs(toolNamesToReplace) do
                        local baseButton = hotbarFrame:FindFirstChild(buttonName) and hotbarFrame[buttonName]:FindFirstChild("Base")
                        if baseButton then
                            local toolName = baseButton:FindFirstChild("ToolName")
                            if toolName and toolName.Text == toolData.original then
                                toolName.Text = toolData.new
                            end
                        end
                    end
                end
            end
        end
        wait()
    end
end
 
checkAndReplaceToolName()
