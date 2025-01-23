--ESP BY CLEITI6966
local espEnabled = false

local function createESP(player)
    if not player.Character or not player.Character:FindFirstChild("Head") then
        return
    end

    local billboard = Instance.new("BillboardGui")
    billboard.Parent = player.Character.Head
    billboard.Size = UDim2.new(5, 0, 1, 0)
    billboard.StudsOffset = Vector3.new(0, 2, 0)
    billboard.Adornee = player.Character.Head
    billboard.AlwaysOnTop = true

    local infoLabel = Instance.new("TextLabel")
    infoLabel.Parent = billboard
    infoLabel.Size = UDim2.new(1, 0, 1, 0)
    infoLabel.BackgroundTransparency = 1
    infoLabel.TextStrokeTransparency = 0.5
    infoLabel.TextColor3 = Color3.new(1, 1, 1)
    infoLabel.Font = Enum.Font.SourceSansBold
    infoLabel.TextSize = 15
    infoLabel.TextScaled = false

    local function updateInfoLabel()
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local distance = (game.Players.LocalPlayer.Character.HumanoidRootPart.Position - player.Character.HumanoidRootPart.Position).Magnitude
            local accountAge = player.AccountAge
            infoLabel.Text = string.format("%s - %d studs - Idade da Conta: %d dias", player.Name, math.floor(distance), accountAge)
        end
    end

    local connection
    connection = game:GetService("RunService").RenderStepped:Connect(function()
        if espEnabled then
            updateInfoLabel()
        else
            connection:Disconnect()
            if billboard.Parent then
                billboard:Destroy()
            end
        end
    end)
end

local function toggleESP()
    espEnabled = not espEnabled
    print("ESP ativado:", espEnabled)  -- VerificaÃ§Ã£o se espEnabled estÃ¡ mudando

    if espEnabled then
        for _, player in pairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer then
                createESP(player)
            end
        end
    else
        for _, player in pairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then
                local billboard = player.Character.Head:FindFirstChildOfClass("BillboardGui")
                if billboard then
                    billboard:Destroy()
                end
            end
        end
    end
end

local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "SH HUB && CLEITI6966 " .. Fluent.Version,
    TabWidth = 130,
    Size = UDim2.fromOffset(580, 460),
    Theme = "blade"
})

local Tabs = {
    Main = Window:AddTab({ Title = "FunÃ§Ãµes", Icon = "home" }),
    Chat = Window:AddTab({ Title = "Chat", Icon = "message-square" }),
    Veiculos = Window:AddTab({ Title = "VeÃ­culos", Icon = "car" }),
    Scripts = Window:AddTab({ Title = "Scripts", Icon = "settings" }),
    Informacoes = Window:AddTab({ Title = "InformaÃ§Ãµes", Icon = "info" }),
}

-- FunÃ§Ãµes (Main Tab)
Tabs.Main:AddParagraph({
    Title = "Scripts",
    Content = "Aqui tem Alguns Scripts Ãšteis Como o Esp e Infinito yield"
})

Tabs.Main:AddButton({
    Title = "Pulos Infinitos",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/HeyGyt/infjump/main/main"))()
    end
})

Tabs.Main:AddButton({
    Title = "Fling",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/0Ben1/fe/main/obf_5wpM7bBcOPspmX7lQ3m75SrYNWqxZ858ai3tJdEAId6jSI05IOUB224FQ0VSAswH.lua.txt", true))()
    end
})

Tabs.Main:AddButton({
    Title = "Infinite Yield",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"))()
    end
})

Tabs.Main:AddButton({
    Title = "ESP",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/Execute666j/TESTE/refs/heads/main/Esp%20do%20sh"))()
    end
})

-- Chat
Tabs.Chat:AddParagraph({
    Title = "Chat Spam",
    Content = "Utilize a funÃ§Ã£o de spam de chat para enviar mensagens repetidamente com um intervalo de tempo."
})

local loopTime = 3
local messageText = "Sua mensagem aqui"
local chatSpamming = false

Tabs.Chat:AddInput("LoopTimeInput", {
    Title = "Tempo do Loop",
    Default = tostring(loopTime),
    Placeholder = "Digite o tempo do loop",
    Numeric = true,
    Finished = true,
    Callback = function(value)
        loopTime = tonumber(value) or loopTime
    end
})

Tabs.Chat:AddInput("MessageInput", {
    Title = "Mensagem",
    Default = messageText,
    Placeholder = "Digite sua mensagem",
    Numeric = false,
    Finished = true,
    Callback = function(value)
        messageText = value
    end
})

local Toggle = Tabs.Chat:AddToggle("ChatSpam", { 
    Title = "Chat Spam", 
    Default = false 
})

Toggle.Callback = function(state)
    chatSpamming = state
    if state then
        task.spawn(function()
            while chatSpamming do
                pcall(function()
                    game:GetService("TextChatService").TextChannels.RBXGeneral:SendAsync(messageText)
                end)
                task.wait(loopTime)
            end
        end)
    end
end

-- CHAT
local settings = {
    AutoChat_Delay = 0,
    AutoChat = false,  -- Por padrÃ£o, o AutoChat comeÃ§a desativado
}

local Players = game:GetService("Players")
local TextChatService = game:GetService("TextChatService")
local localPlayer = Players.LocalPlayer

-- FunÃ§Ã£o para lidar com novas mensagens
local function onIncomingMessage(message: TextChatMessage)
    if settings.AutoChat then
        local speaker = message.TextSource
        
        -- Verifica se nÃ£o Ã© uma mensagem do prÃ³prio jogador
        if speaker and speaker.UserId ~= localPlayer.UserId then
            -- Espera o delay definido
            task.wait(settings.AutoChat_Delay)
            
            -- Repete a mensagem
            TextChatService.TextChannels.RBXGeneral:SendAsync(message.Text)
            print("Mensagem repetida: " .. message.Text)
        end
    end
end

-- Novo Toggle para Repetir o Chat
local ToggleChatRepeat = Tabs.Chat:AddToggle("RepeatChatCLEITI6966", { 
    Title = "Repetir o Chat BY CLEITI6966", 
    Default = false  -- Inicia com o toggle desativado
})

-- Callback para o toggle
ToggleChatRepeat.Callback = function(state)
    settings.AutoChat = state
    if state then
        -- Registra a funÃ§Ã£o de repetiÃ§Ã£o de chat quando o toggle for ativado
        TextChatService.OnIncomingMessage = onIncomingMessage
        print("Repetir o Chat ativado")
    else
        -- Remove a funÃ§Ã£o de repetiÃ§Ã£o de chat quando o toggle for desativado
        TextChatService.OnIncomingMessage = nil
        print("Repetir o Chat desativado")
    end
end

-- VeÃ­culos
Tabs.Veiculos:AddParagraph({
    Title = "Controles de VeÃ­culos",
    Content = "Aqui vocÃª pode personalizar os veÃ­culos e alterar a mÃºsica tocada enquanto dirige."
})

Tabs.Veiculos:AddInput("MusicIDInput", {
    Title = "ID da MÃºsica (Requer Gamepass)",
    Default = "",
    Placeholder = "Digite o ID da mÃºsica",
    Numeric = false,
    Finished = true,
    Callback = function(input)
        local args1 = {
            [1] = "PickingScooterMusicText",
            [2] = input
        }
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args1))

        local args2 = {
            [1] = "PickingCarMusicText",
            [2] = input
        }
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sCa1r"):FireServer(unpack(args2))
    end
})

-- Scripts
Tabs.Scripts:AddParagraph({
    Title = "FunÃ§Ãµes Adicionais",
    Content = "Aqui vocÃª pode acessar funÃ§Ãµes extras como voar, reentrar no servidor, etc."
})

Tabs.Scripts:AddButton({
    Title = "Fly Mobile",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/CLEITI6966/HUB/refs/heads/main/fly.lua"))()
    end
})

Tabs.Scripts:AddButton({
    Title = "VFly Mobile",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/CLEITI6966/HUB/refs/heads/main/vfly.lua"))()
    end
})

Tabs.Scripts:AddButton({
    Title = "Reentrar no server",
    Callback = function()
        loadstring(game:HttpGet("https://pastefy.app/3ESSTYI0/raw"))()
    end
})

local Toggle = Tabs.Scripts:AddToggle("ESP/CLEITI6966", { 
    Title = "ESP/CLEITI6966", 
    Default = false 
})

Toggle:OnChanged(function()
    espEnabled = Toggle.Value
    print("ESP ativado:", espEnabled)

    if espEnabled then
        for _, player in pairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer then
                createESP(player)
            end
        end
    else
        for _, player in pairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then
                local billboard = player.Character.Head:FindFirstChildOfClass("BillboardGui")
                if billboard then
                    billboard:Destroy()
                end
            end
        end
    end
end)

-- Para definir o valor do toggle programaticamente:
Toggle:SetValue(false)  -- Define o toggle como desativado

-- InformaÃ§Ãµes
Tabs.Informacoes:AddParagraph({
    Title = "ATENÃ‡ÃƒOâš ï¸âš ï¸âš ï¸",
    Content = "O SEGUNDO JOGADOR DO RAID CHAT SERVER SE VOCE TIVER UMA CONTA SECUNDÃRIA ONLINE VOCÃŠ PODE EXECUTAR DOIS ROBLOX COM A CONTA SECUNDÃRIA Ã‰ A PRINCIPAL E BOTA O NOME DA CONTA SEGUNDARIA NO SEGUNDO JOGADOR PODE SER AMIGO TAMBÃ‰M E AÃ SÃ“ EXECUTAR O SCRIPT CONTA PRINCIPAL BOTANDO O NOME DA SEGUNDA CONTA Ã‰ NA CONTA SEGUNDARIA BOTA O NOME DA CONTA PRINCIPAL O QUE VAI ACONTECER Ã‰ PARA DEIXAR O RAID CHAT MAIS FORTE DUAS PESSOAS SPAMANDO NO CHAT"
})

-- InformaÃ§Ãµes
Tabs.Informacoes:AddParagraph({
    Title = "ATENÃ‡ÃƒOâš ï¸âš ï¸âš ï¸",
    Content = "SE VOCÃŠ BOTOU NAS DUAS CONTAS Ã‰ QUER BOTAR MAIS UMA TERCEIRA CONTA A TERCEIRA CONTA NO SCRIPT BOTA O NOME DA CONTA SECUNDÃRIA E PRONTO VAI FAZENDO ISSO COM OUTRAS CONTAS DE VOCÃŠ QUISER"
})

-- Aba Troll
local Tabs = {
    Troll = Window:AddTab({ Title = "Trollar", Icon = "troll" }),
}

-- ParÃ¡grafo de AtenÃ§Ã£o
Tabs.Troll:AddParagraph({
    Title = "ATENÃ‡ÃƒOâš ï¸",
    Content = "POR FAVOR SE VOCÃŠ QUER SABER VAI NA ABA INFORMAÃ‡Ã•ES. A INFORMAÃ‡ÃƒO Ã‰ DE QUE SERVER ESSE NEGÃ“CIO DE SEGUNDO JOGADOR.",
})

-- Input para adicionar o nome do segundo jogador
local secondPlayerName = "biungdasilv" -- Valor padrÃ£o

Tabs.Troll:AddInput("SecondPlayerInput", {
    Title = "Adicione o nome do segundo jogador (nome parcial)",
    Default = secondPlayerName,
    Placeholder = "Digite o nome parcial",
    Numeric = false,
    Finished = true,
    Callback = function(value)
        secondPlayerName = value
    end
})

-- VariÃ¡vel de controle do Raid Chat
local raidChatEnabled = false

-- FunÃ§Ã£o para iniciar ou parar o Raid Chat
local function toggleRaidChat(state)
    if state then
        local settings = {
            AutoChat_Delay = 0,
            AutoChat = true,
            IgnoredUser = secondPlayerName
        }

        local Players = game:GetService("Players")
        local TextChatService = game:GetService("TextChatService")
        local localPlayer = Players.LocalPlayer

        local pattern = "â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»â¸»"

        TextChatService.OnIncomingMessage = function(message: TextChatMessage)
            if settings.AutoChat then
                local speaker = message.TextSource

                if speaker and 
                   speaker.UserId ~= localPlayer.UserId and 
                   speaker.Name ~= settings.IgnoredUser then
                    
                    task.wait(settings.AutoChat_Delay)
                    
                    TextChatService.TextChannels.RBXGeneral:SendAsync(pattern)
                    print("PadrÃ£o enviado em resposta a: " .. message.Text)
                end
            end
        end
    else
        -- Desativa o envio automÃ¡tico de chat quando o toggle for desmarcado
        TextChatService.OnIncomingMessage = nil
        print("Raid Chat desativado")
    end
end

-- Toggle para ativar o Raid Chat
Tabs.Troll:AddToggle("RaidChatToggle", {
    Title = "Raid Chat",
    Default = raidChatEnabled,  -- Inicia como desativado
    Callback = function(state)
        raidChatEnabled = state
        toggleRaidChat(state)  -- Atualiza o estado e ativa/desativa o Raid Chat
    end
})

-- Chama a funÃ§Ã£o para garantir que o Raid Chat nÃ£o comece ativado ao iniciar o script
toggleRaidChat(raidChatEnabled)
