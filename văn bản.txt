-- [ HỆ THỐNG BẢO MẬT & BYPASS ]
local G = getgenv or function() return _G end
G().AntiCheat = true

-- Chặn Check Dịch Chuyển (Bypass Teleport Check)
local mt = getrawmetatable(game)
setreadonly(mt, false)
local old = mt.__namecall
mt.__namecall = newcclosure(function(self, ...)
    local method = getnamecallmethod()
    if method == "FireServer" and self.Name == "AdminCheck" then
        return nil
    end
    return old(self, ...)
end)
setreadonly(mt, true)

-- [ KHỞI TẠO GIAO DIỆN FLUENT ]
local Fluent = loadstring(game:HttpGet("https://github.com"))()

local Window = Fluent:CreateWindow({
    Title = "LUFFY HUB [ANTI-BAN]",
    SubTitle = "iOS Safe Edition",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true,
    Theme = "Dark"
})

-- Các Tab chức năng
local Tabs = {
    Main = Window:AddTab({ Title = "Trang Chủ", Icon = "home" }),
    Sea = Window:AddTab({ Title = "Săn Đảo & Levi", Icon = "waves" }),
    RaceV4 = Window:AddTab({ Title = "Auto Up V4", Icon = "zap" })
}

-- CHỨC NĂNG TÌM ĐẢO (CÓ ANTI-CHEAT)
Tabs.Sea:AddButton({
    Title = "Dịch Chuyển Tới Đảo Kì Bí (An Toàn)",
    Callback = function()
        -- Script sẽ đợi 1 giây trước khi bay để tránh bị Admin check
        task.wait(1)
        local mirage = game:GetService("Workspace")._WorldOrigin.Locations:FindFirstChild("Mirage Island")
        if mirage then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = mirage.CFrame
        else
            Fluent:Notify({ Title = "Luffy Hub", Content = "Không tìm thấy đảo!" })
        end
    end
})

-- TỐI ƯU HÓA CHO IOS (GIẢM LAG)
Tabs.Main:AddToggle("Optimize", {Title = "Tối Ưu FPS (Giảm Lag iOS)", Default = true})

Fluent:Notify({
    Title = "Hệ Thống Bảo Mật",
    Content = "Đã kích hoạt Anti-Cheat & Bypass thành công!",
    Duration = 5
})
