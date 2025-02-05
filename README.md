--Stark Versao franca
--Carregar Biblioteca Fluent
local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()


--janela principal
local Window = Fluent:CreateWindow({
    Title = "Starskys Versão Franca-brookhaven🏠" .. Fluent.Version,
    SubTitle = "by Kiwi",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true, -- The blur may be detectable, setting this to false disables blur entirely
    Theme = "Amethyst",
    MinimizeKey = Enum.KeyCode.LeftControl -- Used when theres no MinimizeKeybind
})


--botão De minimizar o Script
local alvodongc
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "Nome"
local Nzx = Instance.new("ImageButton")
ScreenGui.Parent = game.CoreGui

-- Criando o UICorner para deixar o botão redondo
local Estetica = Instance.new("UICorner")
Estetica.CornerRadius = UDim.new(1, 0) -- O "1" faz o botão ficar completamente redondo
Estetica.Parent = Nzx -- Aplica o UICorner ao botão Nzx

Nzx.Name = "Nzx"
Nzx.Parent = ScreenGui
Nzx.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Nzx.Position = UDim2.new(0.799450576, 0, 0.278925627, 0)
Nzx.Size = UDim2.new(0, 50, 0, 50)
Nzx.Visible = true
Nzx.Draggable = true     
Nzx.Active = true     
Nzx.Selectable = true
Nzx.Image = "rbxassetid://" -- Id da imagem aqui

-- Função para alternar a visibilidade de "alvodongc"
for i, v in pairs(game.CoreGui:GetDescendants()) do
    if v.Name == "CanvasGroup" then
        alvodongc = v.Parent
    end
end

local function FVGE_fake_script()
    Nzx.MouseButton1Up:Connect(function()
        task.wait()
        if not toggle then
            toggle = true
            alvodongc.Visible = true
        else
            toggle = false
            alvodongc.Visible = false
        end
    end)
end
coroutine.wrap(FVGE_fake_script)()



--Cria Tab Welcome

local Tab = Window:AddTab({ Title = "Welcome", Icon = "palmtree" })

Tab:AddParagraph({
    Title = "frank version of starskys hub Thank you for using☄️",
    Content = ""
})


--Cria Tab Troll

local Tab = Window:AddTab({ Title = "Troll", Icon = "gamepad" })


Tab:AddParagraph({
    Title = "Fling V19 Toggle",
    Content = ""
})



-- Serviços necessários
local playerService = game:GetService('Players')
local runService = game:GetService('RunService')
local localPlayer = playerService.LocalPlayer
local backpack = localPlayer:FindFirstChildOfClass('Backpack')

-- Variáveis globais
local flingV14Toggle = false
local selectedFlingPlayerV14 = nil
local flingV14Connection
local playerSpawnedConnection

-- Função para obter a lista de jogadores
local function getPlayerList()
    local playerList = {}
    for _, player in ipairs(playerService:GetPlayers()) do
        if player ~= localPlayer then
            table.insert(playerList, player.Name)
        end
    end
    return playerList
end

-- Função para equipar o item "Couch"
local function equipCouch()
    if backpack then
        local couchItem = backpack:FindFirstChild("Couch")
        if couchItem then
            couchItem.Parent = localPlayer.Character
            local humanoid = localPlayer.Character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid:EquipTool(couchItem)
            end
        else
            warn("Couch not found in backpack")
        end
    end
end

-- Função para teleportar para a coordenada
local function teleportToCoordinate()
    local teleportPosition = Vector3.new(-8058744.5, 27872566, -11462984) -- Coordenada para onde você deseja teleportar
    localPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(teleportPosition)
end

-- Função para flingar jogador (V14)
local function flingV14(targetPlayerName)
    local targetPlayer = playerService:FindFirstChild(targetPlayerName)
    if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        -- Looping de teleportes no jogador selecionado
        flingV14Connection = runService.Heartbeat:Connect(function()
            if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                localPlayer.Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
            end

            -- Verifica se o jogador sentou no 'SeatCouch' e realiza o teleporte para a coordenada
            if targetPlayer.Character:FindFirstChild("Humanoid") and targetPlayer.Character.Humanoid.SeatPart then
                teleportToCoordinate()
                flingV14Connection:Disconnect()
            end
        end)
    end
end

-- Função para detectar quando o jogador renasce
local function onPlayerRespawned(targetPlayer)
    if flingV14Toggle then
        -- Aguardar o renascimento do jogador
        playerSpawnedConnection = targetPlayer.CharacterAdded:Connect(function()
            wait(0.0) -- Tempo para garantir que o personagem foi totalmente carregado
            if flingV14Toggle then
                equipCouch() -- Equipar o sofá quando o jogador renasce
                flingV14(targetPlayer.Name)
            end
        end)
    end
end

-- Função para verificar e equipar o sofá caso não esteja equipado
local function ensureCouchEquipped()
    local character = localPlayer.Character
    if character then
        local equipped = false
        for _, tool in ipairs(character:GetChildren()) do
            if tool:IsA("Tool") and tool.Name == "Couch" then
                equipped = true
                break
            end
        end
        if not equipped then
            equipCouch()
        end
    end
end

-- Função de callback para o toggle
local function onFlingV14Toggle(value)
    flingV14Toggle = value
    if flingV14Toggle and selectedFlingPlayerV14 then
        local targetPlayer = playerService:FindFirstChild(selectedFlingPlayerV14)
        if targetPlayer then
            ensureCouchEquipped() -- Equipar o sofá ao ativar o toggle
            flingV14(targetPlayer.Name)
            onPlayerRespawned(targetPlayer)
        end
    elseif not flingV14Toggle then
        -- Desconecta as conexões quando o toggle é desativado
        if flingV14Connection then
            flingV14Connection:Disconnect()
            flingV14Connection = nil
        end
        if playerSpawnedConnection then
            playerSpawnedConnection:Disconnect()
            playerSpawnedConnection = nil
        end
    end
end



local Dropdown = Tab:AddDropdown("Dropdown", {
    Title = "Selecionar Jogando [V19]",
    Values = getPlayerList(),
    Multi = false,
    Default = 1,
})

Dropdown:OnChanged(function(playerName)
    selectedFlingPlayerV14 = playerName
end)

-- Atualiza a lista de jogadores quando os jogadores entram ou saem do jogo
playerService.PlayerAdded:Connect(function(state)
    updateDropdown(playerDropdownV14Toggle)
end)

playerService.PlayerRemoving:Connect(function(state)
    updateDropdown(playerDropdownV14Toggle)
end)


local Toggle = Tab:AddToggle("MyToggle", {Title = "Toggle Fling", Default = false })

Toggle:OnChanged(function(state)
    Callback = onFlingV14Toggle(state)
end)


-- Serviços necessários
local playerService = game:GetService("Players")
local runService = game:GetService("RunService")
local localPlayer = playerService.LocalPlayer
local backpack = localPlayer:FindFirstChildOfClass("Backpack")

-- Variáveis globais
local flingV14Toggle = false
local selectedFlingPlayerV14 = nil
local flingV14Connection
local playerSpawnedConnection

-- Função para obter a lista de jogadores
local function getPlayerList()
    local playerList = {}
    for _, player in ipairs(playerService:GetPlayers()) do
        if player ~= localPlayer then
            table.insert(playerList, player.Name)
        end
    end
    return playerList
end

-- Função para equipar o item "Couch"
local function equipCouch()
    if backpack then
        local couchItem = backpack:FindFirstChild("Couch")
        if couchItem then
            couchItem.Parent = localPlayer.Character
            local humanoid = localPlayer.Character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid:EquipTool(couchItem)
            end
        else
            warn("Couch not found in backpack")
        end
    end
end

-- Função para teleportar para a coordenada
local function teleportToCoordinate()
    local teleportPosition = Vector3.new(-29.709, -498.084, -63.581)
    localPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(teleportPosition)
end

-- Função para flingar jogador (V14)
local function flingV14(targetPlayerName)
    local targetPlayer = playerService:FindFirstChild(targetPlayerName)
    if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        flingV14Connection = runService.Heartbeat:Connect(function()
            if targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                localPlayer.Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
            end
            if targetPlayer.Character:FindFirstChild("Humanoid") and targetPlayer.Character.Humanoid.SeatPart then
                teleportToCoordinate()
                flingV14Connection:Disconnect()
            end
        end)
    end
end

-- Função para detectar renascimento
local function onPlayerRespawned(targetPlayer)
    if flingV14Toggle then
        playerSpawnedConnection = targetPlayer.CharacterAdded:Connect(function()
            wait(0.1)
            if flingV14Toggle then
                equipCouch()
                flingV14(targetPlayer.Name)
            end
        end)
    end
end

-- Função para garantir que o sofá está equipado
local function ensureCouchEquipped()
    local character = localPlayer.Character
    if character then
        local equipped = false
        for _, tool in ipairs(character:GetChildren()) do
            if tool:IsA("Tool") and tool.Name == "Couch" then
                equipped = true
                break
            end
        end
        if not equipped then
            equipCouch()
        end
    end
end

-- Callback para toggle
local function onFlingV14Toggle(value)
    flingV14Toggle = value
    if flingV14Toggle and selectedFlingPlayerV14 then
        local targetPlayer = playerService:FindFirstChild(selectedFlingPlayerV14)
        if targetPlayer then
            ensureCouchEquipped()
            flingV14(targetPlayer.Name)
            onPlayerRespawned(targetPlayer)
        end
    else
        if flingV14Connection then
            flingV14Connection:Disconnect()
            flingV14Connection = nil
        end
        if playerSpawnedConnection then
            playerSpawnedConnection:Disconnect()
            playerSpawnedConnection = nil
        end
    end
end


local Dropdown = Tab:AddDropdown("Dropdown", {
    Title = "Selecione o jogador",
    Values = getPlayerList(),
    Multi = false,
    Default = 1,
})

Dropdown:OnChanged(function(playerName)
    selectedFlingPlayerV14 = playerName
end)



-- Atualizar lista de jogadores dinamicamente
playerService.PlayerAdded:Connect(function()
    Tab:UpdateDropdown({Name = "Selecione o jogador", Options = getPlayerList(state)})
end)

playerService.PlayerRemoving:Connect(function()
    Tab:UpdateDropdown({Name = "Selecione o jogador", Options = getPlayerList(state)})
end)


local Toggle = Tab:AddToggle("MyToggle", {Title = "Ativa Kill Loop", Default = false })

Toggle:OnChanged(function(enable)
    onFlingV14Toggle(enable)
end)


local flingActive = false
local loopFlingActive = false
local selectedPlayer = nil
local bodyVelocity, bodyAngularVelocity

local function startFling()
    if flingActive or not selectedPlayer then return end
    flingActive = true

    local character = game.Players.LocalPlayer.Character
    local targetCharacter = selectedPlayer.Character
    if not character or not targetCharacter then return end
    local hrp = character:WaitForChild("HumanoidRootPart")
    local targetHrp = targetCharacter:WaitForChild("HumanoidRootPart")

    -- Change character size down
    local args = {
        [1] = "CharacterSizeDown",
        [2] = 5
    }
    game:GetService("ReplicatedStorage").RE:FindFirstChild("1Clothe1s"):FireServer(unpack(args))
  
  -- Equipa o item 'Couch'
  local args = {
[1] = "PickingTools",
[2] = "Couch"
}
game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))
  
  -- Equipa o item 'Couch' no inventário se ainda não estiver equipado
local backpack = game.Players.LocalPlayer.Backpack
if backpack and not couchEquipped then
local couch = backpack:FindFirstChild("Couch")
if couch then
game.Players.LocalPlayer.Character.Humanoid:EquipTool(couch)
couchEquipped = true
else
print("O item 'Couch' não foi encontrado no seu inventário.")
end
end
  
    bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.Velocity = Vector3.new(10, 99, 10)
    bodyVelocity.MaxForce = Vector3.new(9999, 9999, 9999)
    bodyVelocity.Parent = hrp

    bodyAngularVelocity = Instance.new("BodyAngularVelocity")
    bodyAngularVelocity.AngularVelocity = Vector3.new(999, 999, 999)
    bodyAngularVelocity.MaxTorque = Vector3.new(999, 999, 999)
    bodyAngularVelocity.Parent = hrp

    game:GetService("RunService").Stepped:Connect(function()
        if flingActive then
            hrp.CFrame = targetHrp.CFrame
            hrp.CFrame = hrp.CFrame * CFrame.Angles(0, math.rad(0), 0)
        end
    end)
end

local function stopFling()
    if not flingActive then return end
    flingActive = false

    -- Change character size up
    local args = {
        [1] = "CharacterSizeUp",
        [2] = 1
    }
    game:GetService("ReplicatedStorage").RE:FindFirstChild("1Clothe1s"):FireServer(unpack(args))

    if bodyVelocity then bodyVelocity:Destroy() end
    if bodyAngularVelocity then bodyAngularVelocity:Destroy() end
end

local function loopFling()
    while loopFlingActive do
        if selectedPlayer and selectedPlayer.Character then
            startFling()
            wait(0.1)
            stopFling()
        end
        wait(0.1)
    end
end

local function updatePlayerList()
local players = game.Players:GetPlayers()
local playerNames = {}
for _, player in ipairs(players) do
table.insert(playerNames, player.Name)
end
return playerNames
end


local Dropdown = Tab:AddDropdown("Dropdown", {
    Title = "Select Player",
    Values = updatePlayerList(),
    Multi = false,
    Default = 1,
})

Dropdown:OnChanged(function(state)
    selectedPlayer = game.Players:FindFirstChild(state)
end)


Tab:AddButton({
   Title = "Start Fling",
   Description = "Very important button",
   Callback = function(state)
       startFling(state)
print("Clicked!")
   end
})



Tab:AddButton({
   Title = "Stop Fling",
   Description = "Very important button",
   Callback = function(state)
     stopFling(state)
  print("Clicked!")
   end
})



Tab:AddButton({
   Title = "Update Player List",
   Description = "Very important button",
   Callback = function(state)
    playerDropdown:Refresh(updatePlayerList(state), true)
   print("Clicked!")
   end
})



Tab:AddParagraph({
    Title = "All(Beta)",
    Content = ""
})


Tab:AddButton({
   Title = "Fling all",
   Description = "Very important button",
   Callback = function()
      loadstring(game:HttpGet("https://pastebin.com/raw/zqyDSUWX"))()
 print("Clicked!")
   end
})


-- Variável para controlar o toggle
local teleportToggle = false

-- Função de teletransporte
local function teleportToPlayers()
    local localPlayer = game.Players.LocalPlayer
    local players = game.Players:GetPlayers()
    
    for _, player in pairs(players) do
        if not teleportToggle then break end -- Para se o toggle for desativado
        if player ~= localPlayer then
        --Ficar pequeno antes
        local args = {
[1] = "CharacterSizeDown",
[2] = 5
}
game:GetService("ReplicatedStorage").RE:FindFirstChild("1Clothe1s"):FireServer(unpack(args))
            -- Teleporta para o jogador
            local character = player.Character
            if character and character:FindFirstChild("HumanoidRootPart") then
                local hrp = character.HumanoidRootPart
                localPlayer.Character.HumanoidRootPart.CFrame = hrp.CFrame
                wait(1) -- Aguarda 1 segundo para evitar problemas de sincronia
            end
        end
        -- Teleporta para (-8.657157897949219, -222.3133087158203, -23.58349609375)
        localPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-8.657157897949219, -222.3133087158203, -23.58349609375)
        wait(1) -- Aguarda antes de passar para o próximo jogador
    end
end



local Toggle = Tab:AddToggle("MyToggle", {Title = "Kill All", Default = false })

Toggle:OnChanged(function(state)
    teleportToggle = state
        if teleportToggle then
            teleportToPlayers(state)
        end
end)



-- Variável para controlar o toggle
local teleportToggle = false

-- Função de teletransporte
local function teleportToSky()


--xaet

local args = {
[1] = "PickingTools",
[2] = "ShoppingCart"
}
game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))

--xaetttttt

local args = {
[1] = "PickingTools",
[2] = "Stretcher"
}
game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))

--Couch

local args = {
[1] = "PickingTools",
[2] = "Couch"
}
game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))

--Equip itens

local function equiparItem(itemName)
    local player = game.Players.LocalPlayer
    local backpack = player:FindFirstChild("Backpack")
    
    if backpack then
        -- Verifica se o item já está no inventário
        local item = backpack:FindFirstChild(itemName)
        if item then
            -- Move o item para a mão do jogador, caso não esteja equipado
            local character = player.Character
            if character and not character:FindFirstChild(itemName) then
                item.Parent = character
                print(itemName .. " equipado!")
            else
                print(itemName .. " já está equipado.")
            end
        else
            print(itemName .. " não está no inventário.")
        end
    else
        print("Backpack não encontrado.")
    end
end

-- Checar e equipar Sniper e FireX
equiparItem("Couch")
equiparItem("ShoppingCart")
equiparItem("Stretcher")

    local localPlayer = game.Players.LocalPlayer
    local players = game.Players:GetPlayers()
    
    for _, player in pairs(players) do
        if not teleportToggle then break end -- Para se o toggle for desativado
        if player ~= localPlayer then
        --Ficar pequeno antes
        local args = {
[1] = "CharacterSizeDown",
[2] = 5
}
game:GetService("ReplicatedStorage").RE:FindFirstChild("1Clothe1s"):FireServer(unpack(args))
            -- Teleporta para o jogador
            local character = player.Character
            if character and character:FindFirstChild("HumanoidRootPart") then
                local hrp = character.HumanoidRootPart
                localPlayer.Character.HumanoidRootPart.CFrame = hrp.CFrame
                wait(1) -- Aguarda 1 segundo para evitar problemas de sincronia
            end
        end
        -- Teleporta para (9999999827968, 9999999827968, 9999999827968)
        localPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(9999999827968, 9999999827968, 9999999827968)
        wait(1) -- Aguarda antes de passar para o próximo jogador
    end
end


local Toggle = Tab:AddToggle("MyToggle", {Title = "Bug all", Default = false })

Toggle:OnChanged(function(state)
    teleportToggle = state
        if teleportToggle then
            teleportToSky(state)
        end
end)


-- Variáveis e serviços
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local TeleportToggle = false
local OriginalPosition

-- Função para teletransportar e voltar à posição original
local function TeleportToPlayers()
    while TeleportToggle do
        -- Salva a posição original do jogador
        if not OriginalPosition then
            OriginalPosition = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart").CFrame
        end
        
        for _, player in ipairs(Players:GetPlayers()) do
            -- Ignora o jogador local
            if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                -- Teleporta para o jogador
                LocalPlayer.Character.HumanoidRootPart.CFrame = player.Character.HumanoidRootPart.CFrame
                wait(1) -- Espera 1 segundo antes de ir para o próximo jogador
                
                -- Retorna para a posição original
                if OriginalPosition then
                    LocalPlayer.Character.HumanoidRootPart.CFrame = OriginalPosition
                end
                wait(1) -- Espera 1 segundo antes de continuar
            end
        end
        
        -- Reseta a posição original se todos foram visitados
        OriginalPosition = nil
    end
end



local Toggle = Tab:AddToggle("MyToggle", {Title = "Bring all", Default = false })

Toggle:OnChanged(function(state)
    TeleportToggle = state
        if TeleportToggle then
            TeleportToPlayers(state)
        end
end)


Tab:AddParagraph({
    Title = "Jail/annoy Player",
    Content = ""
})


local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer


local selectedPlayer = nil
local flingActive = false

local function startFling()
    if selectedPlayer and selectedPlayer.Character and selectedPlayer.Character:FindFirstChild("HumanoidRootPart") then
        -- Configurações básicas de fling
        local bodyGyro = Instance.new("BodyGyro")
        bodyGyro.D = 1000
        bodyGyro.P = 3000
        bodyGyro.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame
        bodyGyro.Parent = LocalPlayer.Character.HumanoidRootPart

        -- Configurações não-parâmetros específicas de fling
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.MaxForce = Vector3.new(1000, 1000, 1000)
        bodyVelocity.Velocity = Vector3.new(0, 100, 0)
        bodyVelocity.Parent = LocalPlayer.Character.HumanoidRootPart

        -- Teleporta o jogador local para a posição do jogador selecionado
        LocalPlayer.Character.HumanoidRootPart.CFrame = selectedPlayer.Character.HumanoidRootPart.CFrame
    end
end

local function stopFling()
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        for _, child in pairs(LocalPlayer.Character.HumanoidRootPart:GetChildren()) do
            if child:IsA("BodyGyro") or child:IsA("BodyVelocity") then
                child:Destroy()
            end
        end
    end
end

local function updatePlayerList()
    local players = game.Players:GetPlayers()
    local playerNames = {}
    for _, player in ipairs(players) do
        table.insert(playerNames, player.Name)
    end
    return playerNames
end




local Dropdown = Tab:AddDropdown("Dropdown", {
    Title = "Selecionar jogando",
    Values = updatePlayerList(),
    Multi = false,
    Default = 1,
})

Dropdown:OnChanged(function(enable)
    selectedPlayer = game.Players:FindFirstChild(enable)
end)


Tab:AddButton({
   Title = "Update Player Lits",
   Description = "Very important button",
   Callback = function(enable)
       playerDropdown:Refresh(updatePlayerList(enable), true)
print("Clicked!")
   end
})


Tab:AddButton({
   Title = "Start Jail V1",
   Description = "Very important button",
   Callback = function(enable)
   flingActive = true
        startFling(enable)
    print("Clicked!")
   end
})



Tab:AddButton({
   Title = "Stop Jail V1",
   Description = "Very important button",
   Callback = function(enable)
     flingActive = false
        stopFling(enable)
  print("Clicked!")
   end
})


RunService.RenderStepped:Connect(function()
    if flingActive and selectedPlayer then
        startFling(enable)
        wait(0.1)
        stopFling(enable)
    end
end)


local players = game:GetService("Players")
local runService = game:GetService("RunService")
local localPlayer = players.LocalPlayer
local camera = game.Workspace.CurrentCamera
local workspace = game.Workspace

-- Variável para armazenar o nome do jogador a ser espectado ou trazido
local targetPlayerName = ""
local targetPlayer

-- Variável para armazenar a posição original do jogador local
local originalPosition = nil

-- Variável para controlar o loop de teletransporte
local bringPlayerLoop = false

-- Função para encontrar jogador por nome completo ou iniciais do primeiro nome com no mínimo 3 caracteres
local function findPlayerByNameOrInitials(input)
    input = input:lower()
    if #input < 3 then
        return nil -- Retorna nil se a entrada for menor que 3 caracteres
    end

    for _, player in pairs(players:GetPlayers()) do
        local playerName = player.Name:lower()
        local playerFirstName = playerName:match("^%S+")
        if playerName == input or playerFirstName:sub(1, #input) == input then
            return player
        end
    end
    return nil
end

-- Função para alterar a câmera para a cabeça do jogador especificado
local function spectatePlayer()
    local targetPlayer = findPlayerByNameOrInitials(targetPlayerName)
    if targetPlayer and targetPlayer.Character then
        local head = targetPlayer.Character:FindFirstChild("Head")
        if head then
            camera.CameraSubject = head
        end
    end
end

-- Função para teleportar o jogador específico para a posição do jogador local
local function bringPlayer()
    targetPlayer = findPlayerByNameOrInitials(targetPlayerName)
    if targetPlayer and targetPlayer.Character and localPlayer.Character then
        originalPosition = localPlayer.Character.HumanoidRootPart.Position

        local function teleport()
            while bringPlayerLoop and targetPlayer and targetPlayer.Character do
                local targetHRP = targetPlayer.Character:FindFirstChild("HumanoidRootPart")
                local seat = targetPlayer.Character:FindFirstChildOfClass("Seat")
                if seat and seat.Occupant then
                    localPlayer.Character:SetPrimaryPartCFrame(CFrame.new(originalPosition))
                    bringPlayerLoop = false
                    break
                end
                if targetHRP then
                    localPlayer.Character:SetPrimaryPartCFrame(targetHRP.CFrame)
                end
                wait(0.1)
            end
        end

        -- Start the teleport loop in a new thread
        spawn(teleport)
    end
end



local Input = Tab:AddInput("Input", {
    Title = "Player Name annoy",
    Default = "Name player",
    Placeholder = "Placeholder",
    Numeric = false, -- Only allows numbers
    Finished = false, -- Only calls callback when you press enter
    Callback = function(state)
        targetPlayerName = state
        print("Player to Target:", targetPlayerName)
    end
})

Input:OnChanged(function()
    print("Input updated:", Input.Value)
end)



local Toggle = Tab:AddToggle("MyToggle", {Title = "Annoy Player", Default = false })

Toggle:OnChanged(function(state)
    bringPlayerLoop = state
        if bringPlayerLoop then
            bringPlayer(state)
        end
end)


Tab:AddParagraph({
    Title = "Ban Kill (Casa 11 Castelo)",
    Content = ""
})



-- Função para teletransportar o jogador local próximo do jogador alvo
function TeleportPlayer(teleportTime, playerName)
    local player = game.Players.LocalPlayer
    local character = player.Character
    if not character then return end

    local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
    if not humanoidRootPart then return end

    local originalPosition = humanoidRootPart.CFrame
    local targetPlayer = nil

    -- Encontra o jogador alvo pelo nome ou nome de exibição
    for _, p in ipairs(game.Players:GetPlayers()) do
        if (string.find(p.Name:lower(), playerName:lower()) or string.find(p.DisplayName:lower(), playerName:lower())) then
            targetPlayer = p
            break
        end
    end

    if not targetPlayer then
        warn("Jogador específico não encontrado.")
        return
    end

    local targetCharacter = targetPlayer.Character
    if targetCharacter and targetCharacter:FindFirstChild("HumanoidRootPart") then
        local targetPosition = targetCharacter.HumanoidRootPart.Position

        -- Função para realizar o "blink" (teleporte rápido)
        local function blink()
            humanoidRootPart.CFrame = CFrame.new(targetPosition + Vector3.new(5, 0, 0))
            wait(0.1)
            humanoidRootPart.CFrame = originalPosition
            wait(0.1)
        end

        local endTime = tick() + teleportTime
        while tick() < endTime do
            blink()
        end
    else
        warn("O jogador específico não está disponível.")
    end
end

-- Serviços do Roblox
local playerService = game:GetService("Players")
local replicatedStorage = game:GetService("ReplicatedStorage")
local localPlayer = playerService.LocalPlayer

-- Função para banir o jogador da "casa"
local function banPlayer(playerName)
    if playerName == localPlayer.Name then
        print("Você não pode se banir da sua própria casa.")
        return
    end

    print("Banindo o jogador:", playerName)
    local targetPlayer = playerService:FindFirstChild(playerName)
    if targetPlayer then
        local args = {"BanPlayerFromHouse", targetPlayer, targetPlayer.Character}
        replicatedStorage.RE:FindFirstChild("1Playe1rTrigge1rEven1t"):FireServer(unpack(args))
    else
        print("Jogador alvo não encontrado.")
    end
end

-- Teletransporta o jogador e realiza o banimento
local function teleportToCoordinateAndBan(playerName)
    if playerName == localPlayer.Name then
        print("Você não pode se banir da sua própria casa.")
        return
    end

    print("Teleportando para a coordenada e banindo...")
    local teleportPosition = Vector3.new(-20, 19, 464)
    localPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(teleportPosition)
    wait(0.2)
    banPlayer(playerName)
end

-- Função de fling e banimento até o jogador sentar
local function flingUntilSeatCouchAndBan(targetPlayerName)
    if targetPlayerName == localPlayer.Name then
        print("Você não pode se banir da sua própria casa.")
        return
    end

    local targetPlayer = playerService:FindFirstChild(targetPlayerName)
    if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local targetPosition = targetPlayer.Character.HumanoidRootPart.CFrame
        isFlinging = true

        while isFlinging do
            -- Movimenta o jogador local ao redor do alvo
            targetPosition = targetPlayer.Character.HumanoidRootPart.CFrame
            localPlayer.Character.HumanoidRootPart.CFrame = targetPosition * CFrame.new(10, 0, 0)
            wait(0)
            localPlayer.Character.HumanoidRootPart.CFrame = targetPosition * CFrame.new(0, 6, 0)
            wait(0)
            localPlayer.Character.HumanoidRootPart.CFrame = targetPosition * CFrame.new(0, 0, 6)
            wait(0)
            localPlayer.Character.HumanoidRootPart.CFrame = targetPosition * CFrame.new(0, 0, -6)
            wait(0)
            localPlayer.Character.HumanoidRootPart.CFrame = targetPosition * CFrame.new(-10, 0, 0)
            wait(0)
            localPlayer.Character.HumanoidRootPart.CFrame = targetPosition * CFrame.new(0, -8, 0)
            wait(0)

            -- Verifica se o jogador alvo sentou
            if targetPlayer.Character.Humanoid.SeatPart then
                isFlinging = false
            end
        end

        teleportToCoordinateAndBan(targetPlayerName)
    else
        print("Jogador alvo não encontrado ou inválido.")
    end
end

-- Monitorar a morte do jogador alvo e reiniciar o banimento
local function monitorPlayerDeath(targetPlayerName)
    while loopBanCouchEnabled do
        local targetPlayer = playerService:FindFirstChild(targetPlayerName)
        if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("Humanoid") then
            if targetPlayer.Character.Humanoid.Health <= 0 then
                print("Jogador alvo morreu. Reiniciando o banimento...")
                flingUntilSeatCouchAndBan(targetPlayerName)
            end
        end
        wait(0)
    end
end

-- Encontra um jogador pelo nome parcial
local function findPlayerByPartialName(partialName)
    partialName = partialName:lower()
    for _, player in ipairs(playerService:GetPlayers()) do
        local playerName = player.Name:lower()
        local firstName = playerName:match("^%w+")
        if (playerName:find(partialName, 1, true) or (firstName and firstName:find(partialName, 1, true))) and (player ~= localPlayer) then
            return player.Name
        end
    end
    return nil
end

-- Previne a morte do jogador local
local function preventDeath()
    if localPlayer.Character and localPlayer.Character:FindFirstChild("Humanoid") then
        local humanoid = localPlayer.Character.Humanoid
        humanoid:GetPropertyChangedSignal("Health"):Connect(function()
            if humanoid.Health < 0 then
                humanoid.Health = 100
            end
        end)
    end
end


local Input = Tab:AddInput("Input", {
    Title = "Input",
    Default = "Default",
    Placeholder = "Placeholder",
    Numeric = false, -- Only allows numbers
    Finished = false, -- Only calls callback when you press enter
    Callback = function(playerName)
   local foundPlayer = findPlayerByPartialName(playerName)
        if foundPlayer then
            selectedBanCouchPlayer = foundPlayer
            print("Jogador selecionado para Ban Couch:", foundPlayer)
            if loopBanCouchEnabled then
                spawn(function() monitorPlayerDeath(selectedBanCouchPlayer) end)
            end
        else
            print("Jogador não encontrado ou você selecionou a si mesmo.")
        end
     print("Input changed:", Value)
    end
})

Input:OnChanged(function()
    print("Input updated:", Input.Value)
end)




Tab:AddButton({
   Title = "ban house",
   Description = "Very important button",
   Callback = function()
   if selectedBanCouchPlayer then
            flingUntilSeatCouchAndBan(selectedBanCouchPlayer)
            preventDeath()
        else
            print("Nenhum jogador selecionado.")
        end
    print("Clicked!")
   end
})


local Tab = Window:AddTab({ Title = "Settings", Icon = "settings" })


Tab:AddParagraph({
    Title = "Kick:Auto report(Manutenção)",
    Content = ""
})


local Players = game:GetService("Players")
local playerNames = {}

local function updatePlayerList()
    playerNames = {}
    for _, player in pairs(Players:GetPlayers()) do
        table.insert(playerNames, player.Name)
    end
end

updatePlayerList()


local selectedPlayer = ""
local selectedReason = ""



local Dropdown = Tab:AddDropdown("Dropdown", {
    Title = "select Jogando",
    Values = playerNames,
    Multi = false,
    Default = 1,
})

Dropdown:OnChanged(function(state)
    selectedPlayer = state
        print("Selected player: " .. state)
end)


Tab:AddButton({
   Title = "Update Player Lits",
   Description = "Very important button",
   Callback = function(state)
   updatePlayerList(state)
    print("Clicked!")
   end
})



local Dropdown = Tab:AddDropdown("Dropdown", {
    Title = "Select Reason",
    Values = {"Web Namoro", "Exploits"},
    Multi = false,
    Default = 1,
})

Dropdown:OnChanged(function(state)
    selectedReason = state
        print("Selected reason: " .. state)
end)


local Toggle = Tab:AddToggle("MyToggle", {Title = "Auto Report active", Default = false })

Toggle:OnChanged(function(Value)
    while value do
            if selectedPlayer ~= "" and selectedReason ~= "" then
                print("Reporting " .. selectedPlayer .. " for " .. selectedReason)
                -- Aqui você pode adicionar o código para enviar a denúncia
            end
            wait(0.1)
        end
end)



Tab:AddParagraph({
    Title = "notifications",
    Content = ""
})


local Players = game:GetService("Players")
local notificationsEnabled = false

local function onPlayerAdded(player)
if notificationsEnabled then
game.StarterGui:SetCore("SendNotification", {
Title = "Player entered";
Text = player.Name .. " entrou no jogo";
Duration = 5;
})
end
end

local function onPlayerRemoving(player)
if notificationsEnabled then
game.StarterGui:SetCore("SendNotification", {
Title = "Player Left";
Text = player.Name .. " left the game";
Duration = 5;
})
end
end

local function toggleNotifications(toggleState)
notificationsEnabled = toggleState
if notificationsEnabled then
print("Notificações Ativadas")
else
print("Notificações Desativadas")
end
end


local Toggle = Tab:AddToggle("MyToggle", {Title = "Notify when player leaves/joins", Default = false })

Toggle:OnChanged(function(state)
    toggleNotifications(state)
end)


Players.PlayerAdded:Connect(onPlayerAdded)
Players.PlayerRemoving:Connect(onPlayerRemoving)


Tab:AddParagraph({
    Title = "Anti function",
    Content = ""
})


local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local TeleportService = game:GetService("TeleportService")

local function disableLaggyFeatures()
for _, v in pairs(workspace:GetDescendants()) do
if v:IsA("ParticleEmitter") or v:IsA("Trail") or v:IsA("Smoke") or v:IsA("Fire") then
v.Enabled = false
end
end
end


local Toggle = Tab:AddToggle("MyToggle", {Title = "Ant Sit (Beta)", Default = false })

Toggle:OnChanged(function(Value)
    if Value then
RunService.Stepped:Connect(function(state)
if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
if LocalPlayer.Character.Humanoid.Sit then
LocalPlayer.Character.Humanoid.Sit = false
end
end
end)
end
end)



local Toggle = Tab:AddToggle("MyToggle", {Title = "Anti Void", Default = false })

Toggle:OnChanged(function(Value)
    if Value then
RunService.Stepped:Connect(function(state)
if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
if LocalPlayer.Character.HumanoidRootPart.Position.Y < -50 then
LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-118.54170227050781, 3.8526408672332764, -1.622152805328369)
end
end
end)
end
end)


local Toggle = Tab:AddToggle("MyToggle", {Title = "Anti Kick", Default = false })

Toggle:OnChanged(function(Value)
    if Value then
game:GetService("CoreGui").RobloxPromptGui.promptOverlay.ChildAdded:Connect(function(child)
if child.Name == "ErrorPrompt" then
TeleportService:Teleport(game.PlaceId, LocalPlayer)
end
end)
end
end)



local Toggle = Tab:AddToggle("MyToggle", {Title = "Anti Lag (Beta)", Default = false })

Toggle:OnChanged(function(Value)
    if Value then
disableLaggyFeatures(state)
workspace.DescendantAdded:Connect(function(descendant)
if descendant:IsA("ParticleEmitter") or descendant:IsA("Trail") or descendant:IsA("Smoke") or descendant:IsA("Fire") then
descendant.Enabled = false
end
end)
end
end)


Tab:AddParagraph({
    Title = "other functions",
    Content = ""
})


local function backToSize()
local args = {
[1] = "CharacterSizeUp",
[2] = 1
}
game:GetService("ReplicatedStorage").RE:FindFirstChild("1Clothe1s"):FireServer(unpack(args)) 
end

local function small()
local args = {
[1] = "CharacterSizeDown",
[2] = 5
}
game:GetService("ReplicatedStorage").RE:FindFirstChild("1Clothe1s"):FireServer(unpack(args))
end


local Dropdown = Tab:AddDropdown("Dropdown", {
    Title = "Select Function - Stay small",
    Values = {"Back to size", "Small"},
    Multi = false,
    Default = 1,
})

Dropdown:OnChanged(function(option)
   if option == "Back to size" then
backToSize()
elseif option == "Small" then
small()
end
end)


local function danger()
local args = {
    [1] = "RolePlayName",
    [2] = "D\205\156\205\137\205\150\204\175A\204\162\204\169\204\150N\205\156\205\153\204\174\204\166\204\173G\204\167\204\150\205\154\205\150\205\149\204\175E\204\167\204\170\205\141\204\153\204\179R\205\162\205\149\204\150\204\171"
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1RPNam1eTex1t"):FireServer(unpack(args))
end

local function hacker()
local args = {
    [1] = "RolePlayName",
    [2] = "H\204\167\204\159\205\150\205\149\204\172\205\148A\204\161\204\172\205\153\205\133\204\174\205\153C\204\168\205\141\204\164\204\150K\204\168\204\174\204\169\204\170E\204\168\204\179\204\169\204\164R\204\161\205\149\205\147\204\163"
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1RPNam1eTex1t"):FireServer(unpack(args))
end

local function invasion()
local args = {
    [1] = "RolePlayName",
    [2] = "I\205\157\204\132\204\128\204\131\204\143n\204\155\205\155\204\132\204\142\204\137v\205\158\204\139\204\137\204\190\205\132a\204\155\204\132\205\131s\205\158\205\140\204\154\204\139\204\133i\204\155\204\130\204\136\204\138\204\136\204\129o\205\161\204\129\204\136n\205\160\205\128\204\129\205\129\204\146\205\132"
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1RPNam1eTex1t"):FireServer(unpack(args))
end


local Dropdown = Tab:AddDropdown("Dropdown", {
    Title = "Select Name - Zalgo",
    Values = {"Danger", "Hacker", "Invasion"},
    Multi = false,
    Default = 1,
})

Dropdown:OnChanged(function(option)
    if option == "Danger" then
danger()
elseif option == "Hacker" then
hacker()
elseif option == "Invasion" then
invasion()
end
end)




local Tab = Window:AddTab({ Title = "Lag", Icon = "plane" })


local function clickNormally(object)
    local clickDetector = object:FindFirstChildWhichIsA("ClickDetector")
    if clickDetector then
        fireclickdetector(clickDetector) -- Clica normalmente
    end
end

local function dupeItem(itemPath, maxTeleports)
    if itemPath then
        local teleportCount = 0
        while teleportCount < maxTeleports do
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = itemPath.CFrame
            clickNormally(itemPath)
            teleportCount = teleportCount + 1
            wait(0.01) -- Tempo de espera para evitar travamento
        end
    end
end

local function lagarServer()
    local ghostMeterPath = workspace:FindFirstChild("WorkspaceCom"):FindFirstChild("001_GiveTools"):FindFirstChild("GhostMeter")
    if ghostMeterPath then
        local teleportCount = 0
        local maxTeleports = 10000000000000000000000

        while teleportCount < maxTeleports do
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = ghostMeterPath.CFrame
            clickNormally(ghostMeterPath)
            teleportCount = teleportCount + 1
            wait(0) -- Executa rapidamente
        end
    else
        warn("Item GhostMeter não encontrado.")
    end
end


Tab:AddButton({
   Title = "Lag Ghost Mater(Op)",
   Description = "Very important button",
   Callback = function(state)
       lagarServer()
print("Clicked!")
   end
})



local toggles = {
    LagGhostMeterV2 = false
}

local function clickNormally(object)
    local clickDetector = object:FindFirstChildWhichIsA("ClickDetector")
    if clickDetector then
        fireclickdetector(clickDetector) -- Clica normalmente
    end
end

local function lagarServer2()
    local ghostMeterPath = workspace:FindFirstChild("WorkspaceCom"):FindFirstChild("001_GiveTools"):FindFirstChild("GhostMeter")
    if ghostMeterPath then
        while toggles.LagGhostMeterV2 do
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = ghostMeterPath.CFrame
            clickNormally(ghostMeterPath)
            wait(0) -- Executa rapidamente
        end
    else
        warn("Item GhostMeter não encontrado.")
    end
end



local Toggle = Tab:AddToggle("MyToggle", {Title = "Lag v2 Toggle Ghost Mater(crash)", Default = false })

Toggle:OnChanged(function(state)
    toggles.LagGhostMeterV2 = state
        if state then
            spawn(lagarServer2) -- Executa o lag em uma nova thread
        else
            print("Lag Ghost Meter V2 desligado.")
        end
end)


local toggles = {
    DupeBasketball = false
}

local function clickNormally(object)
    local clickDetector = object:FindFirstChildWhichIsA("ClickDetector")
    if clickDetector then
        fireclickdetector(clickDetector) -- Clica normalmente
    end
end

local function dupeItem(itemPath, maxTeleports)
    if itemPath then
        local teleportCount = 0
        while teleportCount < maxTeleports and toggles.DupeBasketball do
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = itemPath.CFrame
            clickNormally(itemPath)
            teleportCount = teleportCount + 1
            wait(0.01) -- Tempo de espera para evitar travamento
        end
    else
        warn("Item não encontrado.")
    end
end


local Toggle = Tab:AddToggle("MyToggle", {Title = "Dupe basketball", Default = false })

Toggle:OnChanged(function(state)
 toggles.DupeBasketball = state
        if state then
            local basketballPath = workspace:FindFirstChild("WorkspaceCom"):FindFirstChild("001_GiveTools"):FindFirstChild("Basketball")
            spawn(function()
                dupeItem(basketballPath, 9999999999999999999999999999999999999)
            end)
        else
            print("Dupe Basketball desligado.")
        end
end)


local toggles = {
    DupeExtintor = false
}

local function clickNormally(object)
    local clickDetector = object:FindFirstChildWhichIsA("ClickDetector")
    if clickDetector then
        fireclickdetector(clickDetector) -- Clica normalmente
    end
end

local function dupeItem(itemPath, maxTeleports)
    if itemPath then
        local teleportCount = 0
        while teleportCount < maxTeleports and toggles.DupeExtintor do
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = itemPath.CFrame
            clickNormally(itemPath)
            teleportCount = teleportCount + 1
            wait(0.01) -- Tempo de espera para evitar travamento
        end
    else
        warn("Item não encontrado.")
    end
end


local Toggle = Tab:AddToggle("MyToggle", {Title = "Dupe extinto", Default = false })

Toggle:OnChanged(function(state)
    toggles.DupeExtintor = state
        if state then
            local fireXPath = workspace:FindFirstChild("WorkspaceCom"):FindFirstChild("001_GiveTools"):FindFirstChild("FireX")
            spawn(function()
                dupeItem(fireXPath, 999999999999999999999999999999999999999)
            end)
        else
            print("Dupe Extintor desligado.")
        end
end)



local toggles = {
    DupeBook = false
}

local function clickNormally(object)
    local clickDetector = object:FindFirstChildWhichIsA("ClickDetector")
    if clickDetector then
        fireclickdetector(clickDetector) -- Clica normalmente
    end
end

local function dupeItem(itemPath, maxTeleports)
    if itemPath then
        local teleportCount = 0
        while teleportCount < maxTeleports and toggles.DupeBook do
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = itemPath.CFrame
            clickNormally(itemPath)
            teleportCount = teleportCount + 1
            wait(0.01) -- Tempo de espera para evitar travamento
        end
    else
        warn("Item não encontrado.")
    end
end


local Toggle = Tab:AddToggle("MyToggle", {Title = "Dupe Book", Default = false })

Toggle:OnChanged(function(state)
    toggles.DupeBook = state
        if state then
            local bookPath = workspace:FindFirstChild("WorkspaceCom"):FindFirstChild("001_DayCare"):FindFirstChild("Tools"):FindFirstChild("Book")
            spawn(function()
                dupeItem(bookPath, 99999999999999999999999999999999999)
            end)
        else
            print("Dupe Book desligado.")
        end
end)



local toggles = {
    DupeStretcher = false
}

local function clickNormally(object)
    local clickDetector = object:FindFirstChildWhichIsA("ClickDetector")
    if clickDetector then
        fireclickdetector(clickDetector) -- Clica normalmente
    end
end

local function dupeItem(itemPath, maxTeleports)
    if itemPath then
        local teleportCount = 0
        while teleportCount < maxTeleports and toggles.DupeStretcher do
            if game.Players.LocalPlayer and game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = itemPath.CFrame
                clickNormally(itemPath)
                teleportCount = teleportCount + 1
                wait(0.01) -- Tempo de espera para evitar travamento
            else
                warn("Personagem do jogador não encontrado.")
                break
            end
        end
    else
        warn("ItemPath inválido.")
    end
end

local function dupeStretcher()
    local stretcherPath = workspace:FindFirstChild("WorkspaceCom")
        and workspace.WorkspaceCom:FindFirstChild("001_GiveTools")
        and workspace.WorkspaceCom["001_GiveTools"]:FindFirstChild("Stretcher")
    
    if stretcherPath then
        dupeItem(stretcherPath, 9999999999999999999999999999999999999999999999999999999999) -- Duplicar 300 vezes
    else
        warn("Item Stretcher não encontrado.")
    end
end


local Toggle = Tab:AddToggle("MyToggle", {Title = "Dupe Maca hospital", Default = false })

Toggle:OnChanged(function(state)
    toggles.DupeStretcher = state
        if state then
            spawn(function()
                while toggles.DupeStretcher do
                    dupeStretcher()
                    wait(0.1) -- Espera entre duplicações
                end
            end)
        else
            print("Dupe Stretcher desligado.")
        end
end)


local Tab = Window:AddTab({ Title = "Esp", Icon = "eye" })


local toggles = {
    EspVermelho = false
}

local function activateHighlight()
    local FillColor = Color3.fromRGB(14, 10, 255)
    local DepthMode = "AlwaysOnTop"
    local FillTransparency = 0.5
    local OutlineColor = Color3.fromRGB(14, 10, 255)
    local OutlineTransparency = 0

    local CoreGui = game:FindService("CoreGui")
    local Players = game:FindService("Players")
    local lp = Players.LocalPlayer
    local connections = {}

    local Storage = Instance.new("Folder")
    Storage.Parent = CoreGui
    Storage.Name = "Highlight_Storage"

    local function Highlight(plr)
        local Highlight = Instance.new("Highlight")
        Highlight.Name = plr.Name
        Highlight.FillColor = FillColor
        Highlight.DepthMode = DepthMode
        Highlight.FillTransparency = FillTransparency
        Highlight.OutlineColor = OutlineColor
        Highlight.OutlineTransparency = OutlineTransparency
        Highlight.Parent = Storage

        local plrchar = plr.Character
        if plrchar then
            Highlight.Adornee = plrchar
        end

        connections[plr] = plr.CharacterAdded:Connect(function(char)
            Highlight.Adornee = char
        end)
    end

    Players.PlayerAdded:Connect(Highlight)
    for _, v in next, Players:GetPlayers() do
        Highlight(v)
    end

    Players.PlayerRemoving:Connect(function(plr)
        local plrname = plr.Name
        if Storage:FindFirstChild(plrname) then
            Storage[plrname]:Destroy()
        end
        if connections[plr] then
            connections[plr]:Disconnect()
        end
    end)

    return function()
        if Storage then
            Storage:Destroy()
        end
        for _, conn in pairs(connections) do
            conn:Disconnect()
        end
        connections = {}
    end
end

local deactivateHighlight


local Toggle = Tab:AddToggle("MyToggle", {Title = "Esp Azul", Default = false })

Toggle:OnChanged(function(state)
    toggles.EspVermelho = state
        if state then
            deactivateHighlight = activateHighlight()
        elseif deactivateHighlight then
            deactivateHighlight()
            deactivateHighlight = nil
        end
end)


Tab:AddParagraph({
    Title = "Esp Ver idade da contas Do jogadores+detect Ant sit ",
    Content = ""
})


local toggles = {
    EspIdadeDaConta = false
}

local connections = {}
local function createESP(player)
    if not player.Character or not player.Character:FindFirstChild("Head") then return end

    -- Criar um BillboardGui para mostrar os dias da conta
    local billboardGui = Instance.new("BillboardGui")
    billboardGui.Name = "AccountAgeESP"
    billboardGui.Adornee = player.Character:FindFirstChild("Head")
    billboardGui.Size = UDim2.new(0, 200, 0, 50)
    billboardGui.StudsOffset = Vector3.new(0, 3, 0)
    billboardGui.AlwaysOnTop = true
    
    -- Criar um Label para exibir os dias da conta
    local textLabel = Instance.new("TextLabel")
    textLabel.Parent = billboardGui
    textLabel.Size = UDim2.new(1, 0, 1, 0)
    textLabel.Text = "Dias da conta: " .. tostring(player.AccountAge)
    textLabel.TextColor3 = Color3.new(1, 1, 1)
    textLabel.BackgroundTransparency = 1
    textLabel.Font = Enum.Font.SourceSansBold
    textLabel.TextSize = 14

    -- Adicionar o BillboardGui à cabeça do jogador
    billboardGui.Parent = player.Character:FindFirstChild("Head")
end

local function removeESP(player)
    if player.Character and player.Character:FindFirstChild("Head") then
        local head = player.Character:FindFirstChild("Head")
        local esp = head:FindFirstChild("AccountAgeESP")
        if esp then
            esp:Destroy()
        end
    end
end

local function enableESP()
    -- Adicionar ESP para todos os jogadores no jogo
    for _, player in pairs(game.Players:GetPlayers()) do
        if player.Character and player.Character:FindFirstChild("Head") then
            createESP(player)
        end
    end

    -- Adicionar ESP para novos jogadores que entram no jogo
    connections.PlayerAdded = game.Players.PlayerAdded:Connect(function(player)
        connections[player] = player.CharacterAdded:Connect(function()
            wait(1) -- Espera para garantir que o personagem foi carregado
            if player.Character and player.Character:FindFirstChild("Head") then
                createESP(player)
            end
        end)
    end)

    -- Remover ESP quando o jogador sai
    connections.PlayerRemoving = game.Players.PlayerRemoving:Connect(function(player)
        removeESP(player)
        if connections[player] then
            connections[player]:Disconnect()
            connections[player] = nil
        end
    end)
end

local function disableESP()
    for _, player in pairs(game.Players:GetPlayers()) do
        removeESP(player)
    end

    -- Desconectar todos os eventos
    for _, conn in pairs(connections) do
        if conn then
            conn:Disconnect()
        end
    end
    connections = {}
end


local Toggle = Tab:AddToggle("MyToggle", {Title = "Ativa Esp da idade da conta", Default = false })

Toggle:OnChanged(function(state)
    toggles.EspIdadeDaConta = state
        if state then
            enableESP()
        else
            disableESP()
        end
end)


local toggles = {
    EspToggle = false
}

local function createESP(player)
    if not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") then return end

    local espLabel = Instance.new("BillboardGui")
    espLabel.Name = "ESPLabel"
    espLabel.Adornee = player.Character:FindFirstChild("HumanoidRootPart")
    espLabel.Size = UDim2.new(0, 100, 0, 50)
    espLabel.AlwaysOnTop = true
    espLabel.StudsOffset = Vector3.new(0, 2, 0)
    espLabel.Parent = player.Character:FindFirstChild("HumanoidRootPart")

    local textLabel = Instance.new("TextLabel")
    textLabel.Parent = espLabel
    textLabel.Size = UDim2.new(1, 0, 1, 0)
    textLabel.BackgroundTransparency = 1
    textLabel.TextColor3 = Color3.new(1, 1, 1)
    textLabel.TextScaled = true
    textLabel.TextWrapped = true -- Ajustar tamanho do texto

    local function updateLabel(status)
        textLabel.Text = player.Name .. " - stats: " .. status
    end

    local function updateESP()
        local status = "normal"
        local color = Color3.new(0, 1, 0)
        if player.Character.Humanoid.Sit then
            status = "Sitting"
            color = Color3.new(0, 1, 1)
        elseif player.Character:FindFirstChild("noclip") then
            status = "noclipando"
            color = Color3.new(0.5, 0.25, 0)
        end
        for _, part in pairs(player.Character:GetChildren()) do
            if part:IsA("BasePart") then
                local espBox = part:FindFirstChild("ESP")
                if not espBox then
                    espBox = Instance.new("BoxHandleAdornment")
                    espBox.Name = "ESP"
                    espBox.Adornee = part
                    espBox.Size = part.Size
                    espBox.Transparency = 0.5
                    espBox.AlwaysOnTop = true
                    espBox.ZIndex = 10
                    espBox.Parent = part
                end
                espBox.Color3 = color
            end
        end
        updateLabel(status)
    end

    updateESP()
    
    player.Character.Humanoid:GetPropertyChangedSignal("Sit"):Connect(updateESP)
    player.Character.Humanoid.StateChanged:Connect(updateESP)
    player.Character.HumanoidRootPart:GetPropertyChangedSignal("Velocity"):Connect(updateESP)
    player.Character.ChildAdded:Connect(function(child)
        if child.Name == "noclip" then
            updateESP()
        end
    end)
end

local function activateESP()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer then
            createESP(player)
        end
    end
    game.Players.PlayerAdded:Connect(function(player)
        player.CharacterAdded:Connect(function()
            wait(1)
            if toggles.EspToggle and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                createESP(player)
            end
        end)
    end)
end

local function deactivateESP()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") and player.Character.HumanoidRootPart:FindFirstChild("ESPLabel") then
            player.Character.HumanoidRootPart.ESPLabel:Destroy()
        end
        for _, part in pairs(player.Character:GetChildren()) do
            if part:IsA("BasePart") and part:FindFirstChild("ESP") then
                part.ESP:Destroy()
            end
        end
    end
end


local Toggle = Tab:AddToggle("MyToggle", {Title = "ativa detection Ant sit By:The mr red Black", Default = false })

Toggle:OnChanged(function(state)
    toggles.EspToggle = state
        if state then
            activateESP()
        else
            deactivateESP()
        end
end)


local Tab = Window:AddTab({ Title = "House", Icon = "home" })

Tab:AddParagraph({
    Title = "permissão de casa",
    Content = ""
})


local casaNumbers = {}
for i = 1, 35 do
if i ~= 8 and i ~= 9 and i ~= 10 then
table.insert(casaNumbers, tostring(i))
end
end

local selectedCasaNumber = nil

local Dropdown = Tab:AddDropdown("Dropdown", {
    Title = "Escolha o número da casa",
    Values = casaNumbers,
    Multi = false,
    Default = 1,
})

Dropdown:OnChanged(function(number)
    selectedCasaNumber = tonumber(number)
end)



Tab:AddButton({
   Title = "Pega Permissão",
   Description = "Very important button",
   Callback = function()
      if selectedCasaNumber then
local args = {
[1] = "GivePermissionLoopToServer",
[2] = game:GetService("Players").LocalPlayer,
[3] = selectedCasaNumber
}
game:GetService("ReplicatedStorage").RE:FindFirstChild("1Playe1rTrigge1rEven1t"):FireServer(unpack(args))

else
print("Nenhum número de casa selecionado.")
end
 print("Clicked!")
   end
})
