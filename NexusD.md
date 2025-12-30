wait(1)
local PS=game:GetService("Players");local RS=game:GetService("ReplicatedStorage");local WS=game:GetService("Workspace")
local TS=game:GetService("TweenService");local VU=game:GetService("VirtualUser");local LT=game:GetService("Lighting")
local RSvc=game:GetService("RunService")
local Plr=PS.LocalPlayer;repeat task.wait() until Plr.Character and Plr.Character:FindFirstChild("HumanoidRootPart");task.wait(2)
local Remotes=RS:WaitForChild("Remotes",5);local CF=Remotes:FindFirstChild("CommF_")
local QuestIlha=getgenv().QuestIlha

getgenv().AF=false getgenv().AK=false getgenv().AB=false getgenv().AS=false getgenv().SA="Melee" getgenv().SP=1
getgenv().AR=false getgenv().SR="Human" getgenv().WP="Melee" getgenv().BM=false getgenv().ACF=false getgenv().ABLS=false

Plr.Idled:Connect(function()VU:CaptureController();VU:ClickButton2(Vector2.new())end)

local function GetBelly()local ok,v=pcall(function()return Plr.Data.Beli.Value end)return ok and v or 0 end
local function HasLegendarySword()local bp=Plr.Backpack;local c=Plr.Character;local swords={"Shisui","Wando","Saddi"};for _,n in ipairs(swords)do if bp:FindFirstChild(n)or(c and c:FindFirstChild(n))then return true end end;return false end
local function FindLegendaryDealer()for _,npc in ipairs(WS.NPCs:GetChildren())do if npc.Name=="Legendary Sword Dealer"then return npc end end;return nil end
local function GL()local ok,v=pcall(function()return Plr.Data.Level.Value end)return ok and v or 1 end
local function GS()local hrp=Plr.Character and Plr.Character:FindFirstChild("HumanoidRootPart");if not hrp then return 1 end;local x,z=hrp.Position.X,hrp.Position.Z;if x>=-2000 and x<=2000 and z>=-2000 and z<=2000 then return 1 elseif x>2000 and x<=5000 and z>=-1000 and z<=5000 then return 2 elseif x<-2000 or x>5000 or z<-2000 or z>5000 then return 3 else return 1 end end
local function GQ()if not QuestIlha or not QuestIlha.GetQuestForLevel then return nil end;return QuestIlha.GetQuestForLevel(GL())end
local function HQ()local ok,v=pcall(function()return Plr.PlayerGui.Main.Quest.Visible end)return ok and v end
local function SetNoclipLight(char,enable)if not char then return end;for _,p in ipairs(char:GetDescendants())do if p:IsA("BasePart")then if p.Name=="HumanoidRootPart"or p.Name=="UpperTorso"or p.Name=="LowerTorso"or p.Name=="Torso"then p.CanCollide=not enable end end end end

local Traveling=false IslandTravel=false
local function ApplyFarmPhysics()local c=Plr.Character;local hrp=c and c:FindFirstChild("HumanoidRootPart");local hum=c and c:FindFirstChild("Humanoid");if not c or not hrp or not hum then return end;SetNoclipLight(c,true);hrp.Anchored=false;hrp.AssemblyLinearVelocity=Vector3.new();hum:ChangeState(Enum.HumanoidStateType.RunningNoPhysics)end

function TCF(cf)local c=Plr.Character;local hrp=c and c:FindFirstChild("HumanoidRootPart");if not hrp or not cf then return end;local targetPos=cf.Position;if IslandTravel then targetPos=targetPos+Vector3.new(0,25,0)end;local d=(hrp.Position-targetPos).Magnitude;if d<1 then return end;Traveling=true;SetNoclipLight(c,true);hrp.Anchored=false;hrp.AssemblyLinearVelocity=Vector3.new();local spd=250;local dur=d/spd;local info=TweenInfo.new(dur,Enum.EasingStyle.Linear,Enum.EasingDirection.InOut);local goal={CFrame=CFrame.new(targetPos,hrp.Position+hrp.CFrame.LookVector)};local tw=TS:Create(hrp,info,goal);local hb;hb=RSvc.Heartbeat:Connect(function()if not Traveling or not hrp or not hrp.Parent then return end;hrp.Anchored=false;hrp.AssemblyLinearVelocity=Vector3.new()end);tw.Completed:Connect(function()Traveling=false;if hb then hb:Disconnect()end;if hrp and hrp.Parent then hrp.Anchored=false;hrp.AssemblyLinearVelocity=Vector3.new()end;ApplyFarmPhysics()end);tw:Play();task.wait(dur+0.1);Traveling=false;if hb then hb:Disconnect()end;ApplyFarmPhysics()end

function TIsland(cf)IslandTravel=true;TCF(cf);IslandTravel=false end
function TMob(cf)IslandTravel=false;TCF(cf)end

local function Equip()pcall(function()local c=Plr.Character;if not c or not c:FindFirstChild("Humanoid")then return end;local bp,tool=Plr.Backpack;if getgenv().WP=="Sword"then for _,v in ipairs(bp:GetChildren())do if v:IsA("Tool")and(v.ToolTip=="Sword"or v.Name:find("Katana")or v.Name:find("Blade"))then tool=v break end end end;if not tool then tool=bp:FindFirstChild("Combat")end;if tool then c.Humanoid:EquipTool(tool)end end)end
local function FM(n)local nm,dist=nil,9e9;pcall(function()local c=Plr.Character;local hrp=c and c:FindFirstChild("HumanoidRootPart");if not hrp then return end;for _,m in ipairs(WS.Enemies:GetChildren())do if m.Name==n then local h=m:FindFirstChild("Humanoid");local r=m:FindFirstChild("HumanoidRootPart");if h and r and h.Health>0 then local d=(hrp.Position-r.Position).Magnitude;if d<dist then dist,nm=d,m end end end end end)return nm end
local function GR()local ok,v=pcall(function()return Plr.Data.Race.Value end)return ok and v or"Unknown"end
local function GF()local ok,v=pcall(function()return Plr.Data.Fragments.Value end)return ok and v or 0 end
local function RR()if GF()<3000 or not CF then return false end;local ok=pcall(function()CF:InvokeServer("BlackbeardReward","Reroll","2")end)return ok end
local function FP()pcall(function()LT.GlobalShadows=false;LT.FogEnd=9e9;settings().Rendering.QualityLevel=Enum.QualityLevel.Level01;for _,v in ipairs(WS:GetDescendants())do if v:IsA("BasePart")or v:IsA("UnionOperation")or v:IsA("MeshPart")then v.Material=Enum.Material.Plastic;v.Reflectance=0 elseif v:IsA("Decal")or v:IsA("Texture")then v.Transparency=1 elseif v:IsA("ParticleEmitter")or v:IsA("Trail")then v.Lifetime=NumberRange.new(0)elseif v:IsA("Fire")or v:IsA("SpotLight")or v:IsA("Smoke")or v:IsA("Sparkles")then v.Enabled=false end end end)end
local function AutoHaki()pcall(function()local c=Plr.Character;if not c then return end;if c:FindFirstChild("Buso Haki")or c:FindFirstChild("Enhancement")then return end;local VIM=game:GetService("VirtualInputManager");VIM:SendKeyEvent(true,"J",false,game);task.wait(0.05);VIM:SendKeyEvent(false,"J",false,game)end)end
local function HasNet(p)local ok,o=pcall(function()return p:GetNetworkOwner()end)return ok and(o==Plr or o==nil)end
local function BMob(name,center)if not getgenv().BM or not center then return end;local c=Plr.Character;local hrp=c and c:FindFirstChild("HumanoidRootPart");if not hrp then return end;local list={};for _,m in ipairs(WS.Enemies:GetChildren())do if m.Name==name then local h=m:FindFirstChild("Humanoid");local r=m:FindFirstChild("HumanoidRootPart");if h and r and h.Health>0 and HasNet(r)then local d=(r.Position-hrp.Position).Magnitude;if d<=20 then table.insert(list,{mob=m,root=r,hum=h,dist=d})end end end end;table.sort(list,function(a,b)return a.dist<b.dist end);for i=1,math.min(2,#list)do local data=list[i];local m=data.mob;local r=data.root;local h=data.hum;r.CFrame=CFrame.new(center.Position+Vector3.new(0,-25,0));h.JumpPower=0;h.WalkSpeed=0;r.Transparency=1;r.CanCollide=false;local head=m:FindFirstChild("Head");if head then head.CanCollide=false end;local an=h:FindFirstChild("Animator");if an then an:Destroy()end;if not r:FindFirstChild("Lock")then local bv=Instance.new("BodyVelocity");bv.Name="Lock";bv.MaxForce=Vector3.new(1e5,1e5,1e5);bv.Velocity=Vector3.new();bv.Parent=r end;sethiddenproperty(Plr,"SimulationRadius",math.huge);h:ChangeState(11)end end

local FruitNames={"Bomb-Bomb","Spike-Spike","Chop-Chop","Spring-Spring","Kilo-Kilo","Smoke-Smoke","Flame-Flame","Ice-Ice","Sand-Sand","Dark-Dark","Ghost-Ghost","Magma-Magma","Quake-Quake","Buddha-Buddha","Love-Love","Spider-Spider","Phoenix-Phoenix","Portal-Portal","Rumble-Rumble","Pain-Pain","Blizzard-Blizzard","Gravity-Gravity","Dough-Dough","Shadow-Shadow","Venom-Venom","Control-Control","Spirit-Spirit","Dragon-Dragon","Leopard-Leopard"}
local FruitSet={}for _,n in ipairs(FruitNames)do FruitSet[n]=true end
local function IsFruitModel(m)if not m or not m:IsA("Model")then return false end;if FruitSet[m.Name]then return true end;return m.Name:find("Fruit")~=nil end
local function GetNearestFruit()local my=Plr.Character and Plr.Character:FindFirstChild("HumanoidRootPart");if not my then return end;local best,dist=nil,1e9;for _,m in ipairs(WS:GetChildren())do if IsFruitModel(m)then local p=m:FindFirstChildWhichIsA("BasePart");if p then local d=(p.Position-my.Position).Magnitude;if d<dist then dist,best=d,p end end end end;return best end
local function AnyFruit()for _,m in ipairs(WS:GetChildren())do if IsFruitModel(m)then return true end end;return false end

local ActiveTween=nil;local FollowHB=nil
local function TweenFollow25(hrp,targetHRP,speed)if not hrp or not targetHRP then return end;speed=speed or 300;local yOffset=25;if ActiveTween then ActiveTween:Cancel();ActiveTween=nil end;if FollowHB then FollowHB:Disconnect();FollowHB=nil end;local targetCF=targetHRP.CFrame*CFrame.new(0,yOffset,0);local distance=(hrp.Position-targetCF.Position).Magnitude;local time=math.max(distance/speed,0.05);ActiveTween=TS:Create(hrp,TweenInfo.new(time,Enum.EasingStyle.Linear),{CFrame=targetCF});ActiveTween:Play();ActiveTween.Completed:Once(function()FollowHB=RSvc.Heartbeat:Connect(function()if not targetHRP or not targetHRP.Parent then FollowHB:Disconnect();FollowHB=nil;return end;hrp.CFrame=targetHRP.CFrame*CFrame.new(0,yOffset,0)end)end)end

local Fl=loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local Win=Fl:CreateWindow({Title="Nexus Hub V3",SubTitle="Blox Fruits Delta",TabWidth=160,Size=UDim2.fromOffset(580,460),Theme="Darker"})
local TabM=Win:AddTab({Title="Main",Icon="home"});local TabS=Win:AddTab({Title="Shop",Icon="shopping-bag"})
local TabF=Win:AddTab({Title="Farm",Icon="swords"});local TabSt=Win:AddTab({Title="Status",Icon="trending-up"})
local TabSe=Win:AddTab({Title="Settings",Icon="settings"})

TabM:AddParagraph({Title="Nexus Hub V3",Content="Versao Estavel Delta"})
TabM:AddButton({Title="Discord",Description="Copiar link",Callback=function()setclipboard("https://discord.gg/29BDQqHYbJ")end})

TabS:AddSection("Shop")
TabS:AddButton({Title="Comprar Haki",Description="25k Beli",Callback=function()if CF then CF:InvokeServer("BuyHaki","Buso")end end})
TabS:AddButton({Title="Comprar Race",Description="3k Frags (1x)",Callback=function()RR()end})
TabS:AddDropdown("RaceSel",{Title="Selecionar Race",Values={"Human","Mink","Shark","Angel"},Default=1}):OnChanged(function(v)getgenv().SR=v end)
local TG_AR=TabS:AddToggle("AutoR",{Title="Auto Reroll Race",Description="Para ao conseguir",Default=false})
TG_AR:OnChanged(function(v)getgenv().AR=v end)
local TG_ABLS=TabS:AddToggle("AutoBLS",{Title="Auto Buy Legendary Sword",Description="Compra espada lendária",Default=false})
TG_ABLS:OnChanged(function(v)getgenv().ABLS=v end)

TabF:AddSection("Farming")
TabF:AddDropdown("Weap",{Title="Weapon Style",Values={"Melee","Sword"},Default=1}):OnChanged(function(v)getgenv().WP=v end)
TabF:AddToggle("AF",{Title="Auto Farm Level",Description="0-2800",Default=false}):OnChanged(function(v)getgenv().AF=v;if v then getgenv().AK=false getgenv().AB=false end end)
local TG_AK=TabF:AddToggle("AK",{Title="Auto Katakuri",Description="Sea3 Lv1500+",Default=false})
TG_AK:OnChanged(function(v)if v and(GL()<1500 or GS()~=3)then TG_AK:SetValue(false);return end;getgenv().AK=v;if v then getgenv().AF=false getgenv().AB=false end end)
local TG_AB=TabF:AddToggle("AB",{Title="Auto Bone",Description="Sea3 Lv1500+",Default=false})
TG_AB:OnChanged(function(v)if v and(GL()<1500 or GS()~=3)then TG_AB:SetValue(false);return end;getgenv().AB=v;if v then getgenv().AF=false getgenv().AK=false end end)
TabF:AddToggle("BM",{Title="Bring Mob (2)",Description="Puxa 2 NPC prox.",Default=false}):OnChanged(function(v)getgenv().BM=v end)
TabF:AddToggle("ACF_T",{Title="Auto Collect Fruits",Description="Pega frutas físicas",Default=false}):OnChanged(function(v)getgenv().ACF=v end)

TabSt:AddSlider("Pts",{Title="Pontos por Upgrade",Default=1,Min=1,Max=100,Rounding=0}):OnChanged(function(v)getgenv().SP=v end)
TabSt:AddDropdown("Stat",{Title="Selecionar Status",Values={"Melee","Defense","Sword","Gun","Blox Fruit"},Default=1}):OnChanged(function(v)local m={Melee="Melee",Defense="Defense",Sword="Sword",Gun="Gun",["Blox Fruit"]="Fruit"};getgenv().SA=m[v]end)
TabSt:AddToggle("AS",{Title="Auto Status",Default=false}):OnChanged(function(v)getgenv().AS=v end)

TabSe:AddSection("Server HOP")
TabSe:AddButton({Title="Rejoin",Description="Reconectar",Callback=function()game:GetService("TeleportService"):Teleport(game.PlaceId,Plr)end})
TabSe:AddButton({Title="Hop Server",Description="Poucos players",Callback=function()task.spawn(function()pcall(function()local HS=game:GetService("HttpService");local TS2=game:GetService("TeleportService");local srvs={};local ok,res=pcall(function()return game:HttpGet("https://games.roblox.com/v1/games/"..game.PlaceId.."/servers/Public?sortOrder=Asc&limit=100")end);if ok then local d=HS:JsonDecode(res);if d and d.data then for _,s in ipairs(d.data)do if s.playing and s.playing>=1 and s.playing<=8 and s.id~=game.JobId then table.insert(srvs,s.id)end end end end;if #srvs>0 then TS2:TeleportToPlaceInstance(game.PlaceId,srvs[math.random(1,#srvs)],Plr)else TS2:Teleport(game.PlaceId,Plr)end end)end)end})
TabSe:AddButton({Title="FPS Boost",Description="Tirar Lag",Callback=FP})

local SG=Instance.new("ScreenGui");SG.Name="NexusToggle";SG.ResetOnSpawn=false;SG.Parent=Plr:WaitForChild("PlayerGui")
local BTN=Instance.new("TextButton");BTN.Size=UDim2.fromOffset(60,60);BTN.Position=UDim2.new(0,20,0,20);BTN.BackgroundColor3=Color3.fromRGB(25,25,35);BTN.Text="N";BTN.TextColor3=Color3.fromRGB(255,165,0);BTN.TextSize=18;BTN.Font=Enum.Font.GothamBold;BTN.Parent=SG
local UIC=Instance.new("UICorner");UIC.CornerRadius=UDim.new(0,12);UIC.Parent=BTN;local UIS=Instance.new("UIStroke");UIS.Color=Color3.fromRGB(255,165,0);UIS.Thickness=2;UIS.Parent=BTN
BTN.MouseButton1Click:Connect(function()Win.Root.Visible=not Win.Root.Visible end)

local RA,RH;task.spawn(function()task.wait(2);pcall(function()local M=RS:WaitForChild("Modules",5);if M then local N=M:FindFirstChild("Net");if N then RA=N:FindFirstChild("RE/RegisterAttack");RH=N:FindFirstChild("RE/RegisterHit")end end end)end)

task.spawn(function()while task.wait(0.1)do if(getgenv().AF or getgenv().AK or getgenv().AB)and RA and RH and not Traveling then pcall(function()local tgts,tName={},nil;for _,m in ipairs(WS.Enemies:GetChildren())do if m:FindFirstChild("Head")then if getgenv().AF then local q=GQ();if q and m.Name==(q.M or q.Mon)then table.insert(tgts,{m,m.Head});tName=(q.M or q.Mon)end elseif getgenv().AK and GS()==3 then if m.Name=="Cookie Crafter"or m.Name=="Cake Guard"or m.Name=="Baking Staff"or m.Name=="Head Baker"then table.insert(tgts,{m,m.Head});tName=m.Name end elseif getgenv().AB and GS()==3 then if m.Name=="Reborn Skeleton"or m.Name=="Living Zombie"or m.Name=="Demonic Soul"or m.Name=="Posessed Mummy"then table.insert(tgts,{m,m.Head});tName=m.Name end end end end;if #tgts>0 then local c=Plr.Character;local hrp=c and c:FindFirstChild("HumanoidRootPart");if hrp and getgenv().BM and tName then BMob(tName,hrp.CFrame)end;RA:FireServer(0.1);RH:FireServer(tgts[1][2],tgts)end end)end end end)

task.spawn(function()while task.wait(0.1)do pcall(function()local c=Plr.Character;if not c then return end;local hrp=c:FindFirstChild("HumanoidRootPart");local hum=c:FindFirstChild("Humanoid");if not hrp or not hum then return end;SetNoclipLight(c,true);hum:ChangeState(Enum.HumanoidStateType.RunningNoPhysics);hrp.AssemblyLinearVelocity=Vector3.new();local function farmFlag(flag,mobSingle,listNames)if not getgenv()[flag]then return end;AutoHaki();Equip();local tm;if listNames then for _,n in ipairs(listNames)do tm=FM(n);if tm then break end end else tm=mobSingle end;if not tm or not getgenv()[flag]then return end;local h=tm:FindFirstChild("Humanoid");local r=tm:FindFirstChild("HumanoidRootPart");if not h or not r or h.Health<=0 then return end;TweenFollow25(hrp,r,300);while tm.Parent and h.Health>0 and getgenv()[flag]do hrp.AssemblyLinearVelocity=Vector3.new();Equip();task.wait(0.05)end end;if getgenv().AF then local q=GQ();if not q then return end;local questCF=q.QC or q.CFrameQuest;local mobSpawnCF=q.SC or q.CFrameMon;local questName=q.N or q.NameQuest;local questLevel=q.L or q.LevelQuest;local mobName=q.M or q.Mon;if not HQ()then TIsland(questCF);task.wait(2);if CF then CF:InvokeServer("StartQuest",questName,questLevel)end;task.wait(1)end;local m=FM(mobName);if m and HQ()then farmFlag("AF",m)else TIsland(mobSpawnCF);task.wait(1.5)end elseif getgenv().AK then if GS()==3 and GL()>=1500 then farmFlag("AK",nil,{"Cookie Crafter","Cake Guard","Baking Staff","Head Baker"})else getgenv().AK=false;TG_AK:SetValue(false)end elseif getgenv().AB then if GS()==3 and GL()>=1500 then farmFlag("AB",nil,{"Reborn Skeleton","Living Zombie","Demonic Soul","Posessed Mummy"})else getgenv().AB=false;TG_AB:SetValue(false)end end end)end end)

task.spawn(function()while task.wait(0.4)do pcall(function()if not getgenv().ACF then return end;if not AnyFruit()then return end;local wasAF,wasAK,wasAB=getgenv().AF,getgenv().AK,getgenv().AB;getgenv().AF=false getgenv().AK=false getgenv().AB=false;while AnyFruit()and getgenv().ACF do local fruitPart=GetNearestFruit();if not fruitPart then break end;local c=Plr.Character;local hrp=c and c:FindFirstChild("HumanoidRootPart");if not hrp then break end;IslandTravel=true;TCF(fruitPart.CFrame);IslandTravel=false;local t0=time();while time()-t0<6 do if not fruitPart.Parent then break end;hrp.CFrame=fruitPart.CFrame;task.wait(0.15)end end;if getgenv().ACF then getgenv().AF,getgenv().AK,getgenv().AB=wasAF,wasAK,wasAB end end)end end)

task.spawn(function()while task.wait(1)do if getgenv().AS and getgenv().SA and CF then pcall(function()CF:InvokeServer("AddPoint",getgenv().SA,getgenv().SP)end)end end end)

task.spawn(function()while task.wait(3)do if getgenv().AR and CF then pcall(function()if GR()==getgenv().SR then getgenv().AR=false;TG_AR:SetValue(false);return end;local ok=RR();if not ok then getgenv().AR=false;TG_AR:SetValue(false)end end)end end end)

task.spawn(function()while task.wait(5)do if getgenv().ABLS and CF then pcall(function()if HasLegendarySword()then getgenv().ABLS=false;TG_ABLS:SetValue(false);Fl:Notify({Title="Legendary Sword",Content="Você já possui uma espada lendária!",Duration=5});return end;local dealer=FindLegendaryDealer();if not dealer then Fl:Notify({Title="Legendary Sword",Content="Vendedor não encontrado no servidor!",Duration=5});return end;local belly=GetBelly();if belly<2000000 then Fl:Notify({Title="Legendary Sword",Content="Você precisa de 2M Belly! Atual: "..tostring(belly),Duration=5});return end;local swords={"Shisui","Wando","Saddi"};for _,sword in ipairs(swords)do local success=pcall(function()CF:InvokeServer("BuyItem",sword)end);if success then task.wait(0.5);if HasLegendarySword()then Fl:Notify({Title="Legendary Sword",Content="Espada "..sword.." comprada com sucesso!",Duration=5});getgenv().ABLS=false;TG_ABLS:SetValue(false);return end end;task.wait(0.3)end end)end end end)

Fl:Notify({Title="Nexus Hub V3",Content="Carregado! Sea: "..GS().." | Lv: "..GL(),Duration=5})
