-- زر TDW ON / OFF

local spamGui = Instance.new("ScreenGui", game.CoreGui)

local btn = Instance.new("TextButton", spamGui)

btn.Position = UDim2.new(0.68, 0, 0.50, 0)

btn.Size = UDim2.new(0, 80, 0, 30)

btn.Text = "TDW OFF"

btn.BackgroundColor3 = Color3.fromRGB(101, 67, 33) -- بني

btn.Font = Enum.Font.GothamBold

btn.TextSize = 14

btn.TextColor3 = Color3.fromRGB(255, 0, 0) -- أحمر

Instance.new("UICorner", btn)

local stroke = Instance.new("UIStroke", btn)

stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

stroke.Thickness = 2

stroke.Color = Color3.fromRGB(255, 0, 0) -- أحمر

local attackEnabled = false

local attackingTools = {}

btn.MouseButton1Click:Connect(function()

    attackEnabled = not attackEnabled

    btn.Text = attackEnabled and "TDW ON" or "TDW OFF"

end)

local function activateTool(tool)

    if not attackingTools[tool] then

        attackingTools[tool] = true

        task.spawn(function()

            while attackEnabled and tool.Parent == game.Players.LocalPlayer.Character do

                pcall(function()

                    tool:Activate()

                end)

                task.wait()

            end

            attackingTools[tool] = nil

        end)

    end

end

game:GetService("RunService").Heartbeat:Connect(function()

    if attackEnabled then

        local player = game.Players.LocalPlayer

        if player and player.Character then

            for _, tool in ipairs(player.Backpack:GetChildren()) do

                if tool:IsA("Tool") then

                    if tool.Parent ~= player.Character then

                        tool.Parent = player.Character

                    end

                    activateTool(tool)

                end

            end

        end

    else

        local player = game.Players.LocalPlayer

        if player and player.Character then

            for _, tool in ipairs(player.Character:GetChildren()) do

                if tool:IsA("Tool") then

                    tool.Parent = player.Backpack

                end

            end

        end

        attackingTools = {}

    end

end)
