-- ZF MOD - Onthelpurwadadi | UI redesign
-- Isi: duit live, auto kurir, RACE (bike speed, maks 199), TP gerobak, auto layani bakso, AUTO CLAIM ALL MEME (satu tombol, cepat + stabil), MiniGame Hub (execute/salin),
--      ANTI GAMEPLAY PAUSED, buka/tutup JobGui, delay bisa diatur
-- TAB: FARM | CHAT (tersambung ke web chat baru, login username+password) | SETELAN (grafik super rendah)
-- Tombol buka/tutup bulat berlogo (bisa digeser), window bisa digeser, animasi buka/tutup, GUI tahan hilang
-- TARUH DI: StarterPlayer > StarterPlayerScripts (LocalScript) atau jalankan lewat executor

local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer

------------------------------------------------------------
-- PENGATURAN
------------------------------------------------------------
local LOGO = "rbxassetid://109621974209211"
local PAKAI_COREGUI = true -- true = GUI di gethui/CoreGui supaya tidak ikut hilang saat animasi/cutscene game
local FONT = Enum.Font.Arcade

local function C(r, g, b) return Color3.fromRGB(r, g, b) end
local THEMES = { Ungu = C(138, 43, 226) }
local data = { theme = "Ungu" }

local function new(class, props, parent)
	local o = Instance.new(class)
	for k, v in pairs(props or {}) do o[k] = v end
	o.Parent = parent
	return o
end

local function accent(parent, th)
	return new("UIStroke", {
		Thickness = th or 2, Color = THEMES[data.theme],
		ApplyStrokeMode = Enum.ApplyStrokeMode.Border, LineJoinMode = Enum.LineJoinMode.Miter,
	}, parent)
end

local function label(parent, text, size, pos, dim, color, align)
	return new("TextLabel", {
		BackgroundTransparency = 1, Text = text, Font = FONT, TextSize = size,
		TextColor3 = color or Color3.new(1, 1, 1), Position = pos, Size = dim,
		TextXAlignment = align or Enum.TextXAlignment.Left, TextWrapped = true,
	}, parent)
end

local function outline(l)
	new("UIStroke", { Thickness = 1.5, Color = Color3.new(0, 0, 0) }, l)
end

local function mkBtn(parent, text, pos, size, color)
	local b = new("TextButton", {
		Text = text, Font = FONT, TextSize = 14, TextColor3 = Color3.new(1, 1, 1),
		BackgroundColor3 = color or C(40, 40, 40), BorderSizePixel = 0, Position = pos, Size = size,
	}, parent)
	new("UIStroke", { Color = Color3.new(0, 0, 0), Thickness = 2, LineJoinMode = Enum.LineJoinMode.Miter }, b)
	return b
end

------------------------------------------------------------
-- GUI (tombol bulat + window)
------------------------------------------------------------
local gui, guiHome = nil, nil
do
	local pgui = player:WaitForChild("PlayerGui")
	local function cleanup(parent)
		if not parent then return end
		for _, nm in ipairs({ "ThrowFarm", "MiniGameHub", "ThrowFarmHub", "KerjaHub", "KerjaHubLite", "ZFModOnthelPurwadadi" }) do
			local old = parent:FindFirstChild(nm)
			if old then pcall(function() old:Destroy() end) end
		end
	end
	local function make(parent)
		return new("ScreenGui", {
			Name = "ZFModOnthelPurwadadi", ResetOnSpawn = false, IgnoreGuiInset = true, DisplayOrder = 999999,
			ZIndexBehavior = Enum.ZIndexBehavior.Sibling,
		}, parent)
	end
	cleanup(pgui)
	local target = nil
	if PAKAI_COREGUI then
		local okH, h = pcall(function() return gethui and gethui() end)
		if okH and typeof(h) == "Instance" then target = h end
		if not target then
			local okC, cg = pcall(function() return game:GetService("CoreGui") end)
			if okC and typeof(cg) == "Instance" then target = cg end
		end
	end
	if target then
		cleanup(target)
		local ok, res = pcall(make, target)
		if ok and res and res.Parent then
			gui, guiHome = res, target
		elseif ok and res then
			pcall(function() res:Destroy() end)
		end
	end
	if not gui then
		gui, guiHome = make(pgui), pgui
	end
	gui:GetPropertyChangedSignal("Enabled"):Connect(function()
		if not gui.Enabled then gui.Enabled = true end
	end)
end

local tog = new("ImageButton", {
	Size = UDim2.fromOffset(52, 52), Position = UDim2.new(0, 16, 0.5, -26), BackgroundColor3 = C(32, 20, 48),
	Image = "", ScaleType = Enum.ScaleType.Crop, AutoButtonColor = false,
}, gui)
new("UICorner", { CornerRadius = UDim.new(1, 0) }, tog)
new("TextLabel", {
	BackgroundTransparency = 1, Size = UDim2.fromScale(1, 1), Text = "ZF",
	Font = Enum.Font.GothamBold, TextSize = 16, TextColor3 = Color3.new(1, 1, 1),
	ZIndex = 2,
}, tog)
new("UIAspectRatioConstraint", { AspectRatio = 1 }, tog)
new("UIStroke", { Color = C(138, 43, 226), Thickness = 3, ApplyStrokeMode = Enum.ApplyStrokeMode.Border }, tog)
local togScale = new("UIScale", {}, tog)

local window = new("Frame", {
	Size = UDim2.fromOffset(310, 400), Position = UDim2.fromScale(0.5, 0.5), AnchorPoint = Vector2.new(0.5, 0.5),
	BackgroundTransparency = 1, Visible = false,
}, gui)
local winScale = new("UIScale", { Scale = 0.05 }, window)
accent(window, 3)

local header = new("Frame", {
	Size = UDim2.new(1, 0, 0, 46), BackgroundColor3 = C(55, 28, 82), BorderSizePixel = 0, Active = true,
}, window)
new("UIGradient", { Color = ColorSequence.new(C(138, 43, 226), C(42, 20, 65)), Rotation = 90 }, header)
local logoImg = new("ImageLabel", {
	Size = UDim2.fromOffset(34, 34), Position = UDim2.fromOffset(6, 6), BackgroundColor3 = C(42, 25, 58),
	Image = "", ScaleType = Enum.ScaleType.Crop, BorderSizePixel = 0,
}, header)
new("UICorner", { CornerRadius = UDim.new(1, 0) }, logoImg)
new("TextLabel", {
	BackgroundTransparency = 1, Size = UDim2.fromScale(1, 1), Text = "ZF",
	Font = Enum.Font.GothamBold, TextSize = 12, TextColor3 = Color3.new(1, 1, 1),
	ZIndex = 2,
}, logoImg)
accent(logoImg, 2)
local title = label(header, "ZF MOD", 16, UDim2.fromOffset(48, 4), UDim2.fromOffset(150, 20))
local sub = label(header, "Onthelpurwadadi", 10, UDim2.fromOffset(49, 26), UDim2.fromOffset(190, 14), C(255, 235, 140))
for _, l in ipairs({ title, sub }) do
	l.ZIndex = 2
	outline(l)
end
local closeBtn = mkBtn(header, "X", UDim2.new(1, -36, 0, 9), UDim2.fromOffset(28, 28), C(200, 40, 40))
closeBtn.ZIndex = 3

local farmPage = new("ScrollingFrame", {
	Position = UDim2.fromOffset(0, 76), Size = UDim2.new(1, 0, 1, -76), BackgroundColor3 = C(18, 14, 28),
	BackgroundTransparency = 0.2, BorderSizePixel = 0, ScrollBarThickness = 5,
	AutomaticCanvasSize = Enum.AutomaticSize.Y, CanvasSize = UDim2.new(),
}, window)

-- TAB: FARM | CHAT | SETELAN
local tabBar = new("Frame", {
	Position = UDim2.fromOffset(0, 46), Size = UDim2.new(1, 0, 0, 30), BackgroundColor3 = C(24, 18, 36), BorderSizePixel = 0,
}, window)
local chatPage = new("Frame", {
	Position = UDim2.fromOffset(0, 76), Size = UDim2.new(1, 0, 1, -76), BackgroundColor3 = C(18, 14, 28),
	BackgroundTransparency = 0.2, BorderSizePixel = 0, Visible = false, ClipsDescendants = true,
}, window)
local setPage = new("ScrollingFrame", {
	Position = UDim2.fromOffset(0, 76), Size = UDim2.new(1, 0, 1, -76), BackgroundColor3 = C(18, 14, 28),
	BackgroundTransparency = 0.2, BorderSizePixel = 0, ScrollBarThickness = 5, Visible = false,
	AutomaticCanvasSize = Enum.AutomaticSize.Y, CanvasSize = UDim2.new(),
}, window)
local tabPages = { FARM = farmPage, CHAT = chatPage, SETELAN = setPage }
local tabBtns = {}
local function showTab(name)
	for n, p in pairs(tabPages) do p.Visible = (n == name) end
	for n, b in pairs(tabBtns) do
		b.BackgroundColor3 = (n == name) and C(138, 43, 226) or C(48, 30, 64)
	end
end
for i, name in ipairs({ "FARM", "CHAT", "SETELAN" }) do
	local b = mkBtn(tabBar, name, UDim2.new((i - 1) / 3, 3, 0, 3), UDim2.new(1 / 3, -6, 1, -6), C(48, 30, 64))
	b.TextSize = 13
	tabBtns[name] = b
	b.Activated:Connect(function() showTab(name) end)
end
showTab("FARM")

------------------------------------------------------------
-- DRAG + ANIMASI BUKA/TUTUP + PENJAGA
------------------------------------------------------------
local function makeDraggable(handle, target)
	local drag, moved, start, startPos = false, false, nil, nil
	handle.InputBegan:Connect(function(i)
		if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
			drag, moved = true, false
			start = i.Position
			startPos = target.AbsolutePosition + target.AbsoluteSize * target.AnchorPoint
		end
	end)
	UIS.InputChanged:Connect(function(i)
		if drag and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then
			local dx, dy = i.Position.X - start.X, i.Position.Y - start.Y
			if not moved and math.abs(dx) + math.abs(dy) < 6 then return end
			moved = true
			local vp, sz, ap = gui.AbsoluteSize, target.AbsoluteSize, target.AnchorPoint
			local x = math.clamp(startPos.X + dx, sz.X * ap.X, math.max(sz.X * ap.X, vp.X - sz.X * (1 - ap.X)))
			local y = math.clamp(startPos.Y + dy, sz.Y * ap.Y, math.max(sz.Y * ap.Y, vp.Y - sz.Y * (1 - ap.Y)))
			target.Position = UDim2.fromOffset(x, y)
		end
	end)
	UIS.InputEnded:Connect(function(i)
		if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then drag = false end
	end)
	return function()
		local m = moved
		moved = false
		return m
	end
end
local tbMoved = makeDraggable(tog, tog)
makeDraggable(header, window)

local isOpen = false
local winTween, togTween = nil, nil
local function setOpen(open)
	if open == isOpen then return end
	isOpen = open
	if winTween then winTween:Cancel() end
	if togTween then togTween:Cancel() end
	togTween = TweenService:Create(tog, TweenInfo.new(0.4, Enum.EasingStyle.Back, Enum.EasingDirection.Out), { Rotation = open and 360 or 0 })
	togTween:Play()
	if open then
		local vp = gui.AbsoluteSize
		if vp.X < 50 and workspace.CurrentCamera then vp = workspace.CurrentCamera.ViewportSize end
		window.Size = UDim2.fromOffset(math.clamp(vp.X * 0.9, 280, 320), math.clamp(vp.Y * 0.88, 260, 440))
		window.Visible = true
		winScale.Scale = 0.05
		winTween = TweenService:Create(winScale, TweenInfo.new(0.35, Enum.EasingStyle.Back, Enum.EasingDirection.Out), { Scale = 1 })
		winTween:Play()
	else
		local tw = TweenService:Create(winScale, TweenInfo.new(0.22, Enum.EasingStyle.Back, Enum.EasingDirection.In), { Scale = 0.05 })
		winTween = tw
		tw:Play()
		tw.Completed:Connect(function()
			if not isOpen then window.Visible = false end
		end)
	end
end

tog.InputBegan:Connect(function(i)
	if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
		TweenService:Create(togScale, TweenInfo.new(0.1), { Scale = 0.9 }):Play()
	end
end)
tog.InputEnded:Connect(function(i)
	if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
		TweenService:Create(togScale, TweenInfo.new(0.15, Enum.EasingStyle.Back, Enum.EasingDirection.Out), { Scale = 1 }):Play()
	end
end)
tog.Activated:Connect(function()
	if tbMoved() then return end
	setOpen(not isOpen)
end)
closeBtn.Activated:Connect(function() setOpen(false) end)

-- PENJAGA: GUI tidak boleh hilang walau game memutar animasi / cutscene
do
	local dead = false
	gui.Destroying:Connect(function() dead = true end)
	tog:GetPropertyChangedSignal("Visible"):Connect(function()
		if not tog.Visible then tog.Visible = true end
	end)
	task.spawn(function()
		while not dead do
			task.wait(0.4)
			if gui.Parent ~= guiHome then
				pcall(function() gui.Parent = guiHome end)
			end
			if not gui.Enabled then gui.Enabled = true end
			if not tog.Visible then tog.Visible = true end
		end
	end)
end

------------------------------------------------------------
-- CHAT (tab sendiri) - tersambung ke web chat (Supabase)
-- Butuh executor (HTTP). Chat kamu di kanan, chat orang di kiri.
------------------------------------------------------------
do
	local HttpService = game:GetService("HttpService")
	local SUPABASE_URL = "https://gtntmadknqaslqzizvqt.supabase.co"
	local ANON_KEY = "sb_publishable_G0A0O_OP_K2rMgRSayUKgw__csYYCvU"
	local EMAIL_DOMAIN = "@sharescript-chat.com" -- harus sama dengan di web
	local SAVE_FILE = "webchat_session.txt"

	local function httpRequest(method, path, body, token, extra)
		local headers = { ["apikey"] = ANON_KEY, ["Content-Type"] = "application/json" }
		if token then headers["Authorization"] = "Bearer " .. token end
		if extra then for k, v in pairs(extra) do headers[k] = v end end
		local req = {
			Url = SUPABASE_URL .. path,
			Method = method,
			Headers = headers,
			Body = body and HttpService:JSONEncode(body) or nil,
		}
		local fn = (syn and syn.request) or http_request or request or (fluxus and fluxus.request)
		local ok, res = pcall(function()
			if fn then return fn(req) end
			return HttpService:RequestAsync(req)
		end)
		if not ok or type(res) ~= "table" then return nil, 0 end
		local data
		if res.Body and res.Body ~= "" then
			local ok2, d = pcall(function() return HttpService:JSONDecode(res.Body) end)
			if ok2 then data = d end
		end
		return data, res.StatusCode or 0
	end
	local function isOk(code) return code and code >= 200 and code < 300 end

	local currentUsername, accessToken, refreshToken = nil, nil, nil

	-- simpan / muat sesi (password TIDAK disimpan, cuma refresh token)
	local function saveSession()
		if not currentUsername or not refreshToken then return end
		local s = currentUsername .. "|" .. refreshToken
		pcall(function() getgenv().WebChatSession = s end)
		pcall(function() writefile(SAVE_FILE, s) end)
	end
	local function clearSession()
		pcall(function() getgenv().WebChatSession = nil end)
		pcall(function() writefile(SAVE_FILE, "") end)
	end
	local function parseSession(str)
		if type(str) ~= "string" then return nil end
		str = str:gsub("%s", "")
		return str:match("^([a-z0-9_]+)|(.+)$")
	end
	local function loadSession()
		local ok, data = pcall(function() return readfile(SAVE_FILE) end)
		if ok and parseSession(data) then return parseSession(data) end
		local ok2, g = pcall(function() return getgenv().WebChatSession end)
		if ok2 and parseSession(g) then return parseSession(g) end
		return nil
	end

	local function refreshSession()
		if not refreshToken then return false end
		local data, code = httpRequest("POST", "/auth/v1/token?grant_type=refresh_token", { refresh_token = refreshToken })
		if isOk(code) and type(data) == "table" and data.access_token then
			accessToken = data.access_token
			refreshToken = data.refresh_token or refreshToken
			saveSession()
			return true
		end
		return false
	end

	local function authenticate(mode, name, pass)
		local email = name .. EMAIL_DOMAIN
		local data, code
		if mode == "reg" then
			data, code = httpRequest("POST", "/auth/v1/signup", { email = email, password = pass, data = { username = name } })
		else
			data, code = httpRequest("POST", "/auth/v1/token?grant_type=password", { email = email, password = pass })
		end
		if isOk(code) and type(data) == "table" and data.access_token then
			currentUsername = name
			accessToken = data.access_token
			refreshToken = data.refresh_token
			saveSession()
			return true
		end
		local msg = ""
		if type(data) == "table" then
			msg = tostring(data.msg or data.message or data.error_description or data.error or "")
		end
		local low = msg:lower()
		if isOk(code) and type(data) == "table" and not data.access_token then
			return false, "Akun dibuat tapi perlu konfirmasi email. Matikan 'Confirm email' di Supabase."
		elseif low:find("already") or low:find("registered") or low:find("database error") then
			return false, "@" .. name .. " sudah dipakai atau tidak valid, coba yang lain."
		elseif low:find("invalid login") then
			return false, "Username atau password salah."
		elseif low:find("rate limit") then
			return false, "Terlalu sering mencoba, tunggu sebentar."
		elseif code == 0 then
			return false, "Gagal terhubung. Chat butuh executor dengan HTTP."
		end
		return false, (msg ~= "" and msg or "Gagal") .. " (kode " .. tostring(code) .. ")"
	end

	-- panel login
	local loginPanel = new("Frame", { Size = UDim2.fromScale(1, 1), BackgroundTransparency = 1 }, chatPage)
	label(loginPanel, "Daftar sekali (username tidak bisa diganti) atau masuk pakai akun yang sama dengan web chat.", 11,
		UDim2.fromOffset(10, 6), UDim2.new(1, -20, 0, 40), C(180, 200, 255))
	local userBox = new("TextBox", {
		Position = UDim2.fromOffset(10, 50), Size = UDim2.new(1, -20, 0, 34), BackgroundColor3 = C(24, 18, 36), BorderSizePixel = 0,
		Text = "", PlaceholderText = "username (a-z, 0-9, _ ; 3-20)", PlaceholderColor3 = C(130, 150, 190),
		Font = FONT, TextSize = 14, TextColor3 = C(255, 225, 90), ClearTextOnFocus = false,
	}, loginPanel)
	accent(userBox, 1)
	local passBox = new("TextBox", {
		Position = UDim2.fromOffset(10, 90), Size = UDim2.new(1, -20, 0, 34), BackgroundColor3 = C(24, 18, 36), BorderSizePixel = 0,
		Text = "", PlaceholderText = "password (min 6)", PlaceholderColor3 = C(130, 150, 190),
		Font = FONT, TextSize = 14, TextColor3 = Color3.new(1, 1, 1), ClearTextOnFocus = false,
	}, loginPanel)
	accent(passBox, 1)
	local regBtn = mkBtn(loginPanel, "DAFTAR", UDim2.fromOffset(10, 132), UDim2.new(0.5, -15, 0, 36), C(40, 150, 80))
	local logBtn = mkBtn(loginPanel, "MASUK", UDim2.new(0.5, 5, 0, 132), UDim2.new(0.5, -15, 0, 36), C(45, 110, 220))
	local loginSt = label(loginPanel, "", 11, UDim2.fromOffset(10, 176), UDim2.new(1, -20, 0, 70), C(255, 120, 120))

	do
		local okW = pcall(function() writefile("webchat_test.txt", "ok") end)
		local okR, v = pcall(function() return readfile("webchat_test.txt") end)
		if okW and okR and v == "ok" then
			loginSt.TextColor3 = C(120, 220, 150)
			loginSt.Text = "Penyimpanan file: OK (login tersimpan otomatis)"
		else
			loginSt.TextColor3 = C(255, 200, 50)
			loginSt.Text = "Penyimpanan file tidak didukung, kamu harus masuk lagi tiap kali."
		end
	end

	-- panel chat
	local chatPanel = new("Frame", { Size = UDim2.fromScale(1, 1), BackgroundTransparency = 1, Visible = false }, chatPage)
	local chatSt = label(chatPanel, "", 11, UDim2.fromOffset(8, 3), UDim2.new(1, -76, 0, 14), C(120, 255, 150))
	local logoutBtn = mkBtn(chatPanel, "KELUAR", UDim2.new(1, -66, 0, 1), UDim2.fromOffset(60, 18), C(150, 50, 50))
	local msgs = new("ScrollingFrame", {
		Position = UDim2.fromOffset(6, 22), Size = UDim2.new(1, -12, 1, -70), BackgroundColor3 = C(24, 18, 36),
		BackgroundTransparency = 0.2, BorderSizePixel = 0, ScrollBarThickness = 4,
		AutomaticCanvasSize = Enum.AutomaticSize.Y, CanvasSize = UDim2.new(),
	}, chatPanel)
	new("UIListLayout", { Padding = UDim.new(0, 4), SortOrder = Enum.SortOrder.LayoutOrder }, msgs)
	new("UIPadding", {
		PaddingTop = UDim.new(0, 5), PaddingBottom = UDim.new(0, 5), PaddingLeft = UDim.new(0, 5), PaddingRight = UDim.new(0, 7),
	}, msgs)
	local inputBox = new("TextBox", {
		Position = UDim2.new(0, 6, 1, -42), Size = UDim2.new(1, -82, 0, 36), BackgroundColor3 = C(24, 18, 36), BorderSizePixel = 0,
		Text = "", PlaceholderText = "Ketik pesan...", PlaceholderColor3 = C(130, 150, 190),
		Font = FONT, TextSize = 14, TextColor3 = Color3.new(1, 1, 1), ClearTextOnFocus = false, TextXAlignment = Enum.TextXAlignment.Left,
	}, chatPanel)
	accent(inputBox, 1)
	local sendBtn = mkBtn(chatPanel, "KIRIM", UDim2.new(1, -72, 1, -42), UDim2.fromOffset(66, 36), C(40, 150, 80))

	local lastId, fetching, count = 0, false, 0
	local savedScroll = nil

	-- ingat posisi scroll pas CHAT atau jendela ditutup, kembalikan pas dibuka lagi
	-- (ScrollingFrame bisa lompat ke atas saat frame-nya disembunyikan lalu ditampilkan lagi)
	chatPage:GetPropertyChangedSignal("Visible"):Connect(function()
		if not chatPage.Visible then
			savedScroll = msgs.CanvasPosition
		elseif savedScroll then
			task.defer(function() msgs.CanvasPosition = savedScroll end)
		end
	end)
	window:GetPropertyChangedSignal("Visible"):Connect(function()
		if not window.Visible then
			savedScroll = msgs.CanvasPosition
		elseif savedScroll and chatPage.Visible then
			task.defer(function() msgs.CanvasPosition = savedScroll end)
		end
	end)

	local TS = game:GetService("TweenService")
	local CHAT_FONT = Enum.Font.Code

	local function addMessage(m, animate)
		count 
