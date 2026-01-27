> 由gemini-3pro将talkfropreset.md文件内容翻译成中文
> 文档翻译日期为1月26日，需要注意时效性
> rimtalk的项目地址： https://github.com/jlibrary/RimTalk
> 建议结合挖掘代码的人的帖子来看： https://tieba.baidu.com/p/10408667645?pid=153105380480&cid=#153105380480


这里是该文档的中文翻译版本。我保留了代码块、变量名（`{{ ... }}`）和技术性较强的类型标识（如 `bool`, `int` 等），仅将解释性文本、标题、注释和说明栏进行了翻译，以便于阅读和理解。

--- START OF FILE preset.md ---

# RimTalk Scriban 示例（已知可用变量）

{{- # ===============================
    # 1) 系统 / 上下文
    # =============================== -}}
语言: {{ lang }}
当前小时（游戏内）: {{ hour }}
JSON 格式说明:
{{ json.format }}

聊天记录:
{{ chat.history }}

对话类型: {{ ctx.DialogueType }}
对话状态: {{ ctx.DialogueStatus }}
是否独白: {{ ctx.IsMonologue }}
用户原始输入（仅限玩家对话）: {{ ctx.UserPrompt }}

修饰后的提示词:
{{ prompt }}

Pawn 上下文（简短）:
{{ context }}

{{- # ===============================
    # 2) 主要 Pawn（发起者）简写
    # =============================== -}}
Pawn: {{ pawn.name }}
角色: {{ pawn.role }}
当前活动: {{ pawn.job }}
心情: {{ pawn.mood }}
性格: {{ pawn.personality }}
社交: {{ pawn.social }}
位置: {{ pawn.location }}
美观度: {{ pawn.beauty }}
清洁度: {{ pawn.cleanliness }}
周边环境: {{ pawn.surroundings }}

{{- # ===============================
    # 3) 接收者
    # =============================== -}}
接收者: {{ recipient.name }}
接收者角色: {{ recipient.role }}

{{- # ===============================
    # 4) 所有 Pawn (pawns)
    # =============================== -}}
{{- for p in pawns -}}
- {{ p.name }} ({{ p.role }}) / {{ p.job }} / {{ p.mood }}
  位置: {{ p.location }}
  社交: {{ p.social }}
{{- end -}}

{{- # ===============================
    # 5) 地图
    # =============================== -}}
当前天气: {{ map.weather }}
当前气温: {{ map.temperature }}

{{- # ===============================
    # 6) 工具函数 (PawnUtil / CommonUtil)
    # =============================== -}}
{{- if (IsTalkEligible pawn) -}}
是否有资格对话: 是
{{- else -}}
是否有资格对话: 否
{{- end -}}
角色 (工具类获取): {{ GetRole pawn }}

{{- # ===============================
    # 7) 静态类（谨慎使用）
    # =============================== -}}
绝对 Ticks 数: {{ Find.TickManager.TicksAbs }}
当前小时 (GenDate): {{ GenDate.HourOfDay(Find.TickManager.TicksAbs, 0) }}
地图数量 (所有地图): {{ PawnsFinder.AllMaps.Count }}

# Pawn Scriban 字段（一级，可直接用于提示词）
注意：
- 一级字段使用 `pawn.<field>`，且必须能渲染为文本/数字/布尔值。
- 仅列出了用于提示词生成时安全的字段。

| 字段 | 示例 | 说明 |
| --- | --- | --- |
| name | {{ pawn.name }} | 特殊属性: LabelShort |
| job | {{ pawn.job }} | 特殊属性 |
| role | {{ pawn.role }} | 特殊属性 |
| mood | {{ pawn.mood }} | 特殊属性 |
| personality | {{ pawn.personality }} | 特殊属性 |
| social | {{ pawn.social }} | 特殊属性 |
| location | {{ pawn.location }} | 特殊属性 |
| beauty | {{ pawn.beauty }} | 特殊属性 |
| cleanliness | {{ pawn.cleanliness }} | 特殊属性 |
| surroundings | {{ pawn.surroundings }} | 特殊属性 |
| KindLabel | {{ pawn.KindLabel }} | string |
| LabelShort | {{ pawn.LabelShort }} | string |
| LabelCap | {{ pawn.LabelCap }} | string |
| Downed | {{ pawn.Downed }} | bool (是否倒地) |
| Dead | {{ pawn.Dead }} | bool (是否死亡) |
| DeadOrDowned | {{ pawn.DeadOrDowned }} | bool (死或倒地) |
| InMentalState | {{ pawn.InMentalState }} | bool (精神状态中) |
| InAggroMentalState | {{ pawn.InAggroMentalState }} | bool (攻击性精神状态中) |
| IsColonist | {{ pawn.IsColonist }} | bool (是殖民者) |
| IsFreeColonist | {{ pawn.IsFreeColonist }} | bool (是自由殖民者) |
| IsPrisoner | {{ pawn.IsPrisoner }} | bool (是囚犯) |
| IsSlave | {{ pawn.IsSlave }} | bool (是奴隶) |
| IsColonistPlayerControlled | {{ pawn.IsColonistPlayerControlled }} | bool (玩家控制的殖民者) |
| IsColonyAnimal | {{ pawn.IsColonyAnimal }} | bool (殖民地动物) |
| IsColonyMech | {{ pawn.IsColonyMech }} | bool (殖民地机械体) |
| IsMutant | {{ pawn.IsMutant }} | bool (是变种人) |
| IsGhoul | {{ pawn.IsGhoul }} | bool (是食尸鬼) |
| IsShambler | {{ pawn.IsShambler }} | bool (是蹒跚者) |
| IsSubhuman | {{ pawn.IsSubhuman }} | bool (是亚人) |
| IsCreepJoiner | {{ pawn.IsCreepJoiner }} | bool (是怪诞加入者) |
| BodySize | {{ pawn.BodySize }} | float (体型大小) |
| HealthScale | {{ pawn.HealthScale }} | float (健康规模) |

# Pawn Scriban 字段（二级，可直接用于提示词）
注意：
- 二级字段使用 `pawn.<field>.<subfield>`。
- 仅限管理器内部使用或非文本类型的字段已排除。

| 字段 | 示例 | 说明 |
| --- | --- | --- |
| RaceProps.Humanlike | {{ pawn.RaceProps.Humanlike }} | bool (类人生物) |
| RaceProps.IsMechanoid | {{ pawn.RaceProps.IsMechanoid }} | bool (机械体) |
| RaceProps.intelligence | {{ pawn.RaceProps.intelligence }} | enum (智力等级) |
| ageTracker.AgeBiologicalYears | {{ pawn.ageTracker.AgeBiologicalYears }} | int (生物学年龄-年) |
| ageTracker.AgeBiologicalYearsFloat | {{ pawn.ageTracker.AgeBiologicalYearsFloat }} | float |
| ageTracker.AgeChronologicalYears | {{ pawn.ageTracker.AgeChronologicalYears }} | int (纪年年龄-年) |
| ageTracker.AgeChronologicalYearsFloat | {{ pawn.ageTracker.AgeChronologicalYearsFloat }} | float |
| ageTracker.AgeNumberString | {{ pawn.ageTracker.AgeNumberString }} | string |
| ageTracker.BirthYear | {{ pawn.ageTracker.BirthYear }} | int |
| ageTracker.BirthDayOfYear | {{ pawn.ageTracker.BirthDayOfYear }} | int |
| ageTracker.BirthQuadrum | {{ pawn.ageTracker.BirthQuadrum }} | enum (出生季度) |
| ageTracker.CurLifeStage | {{ pawn.ageTracker.CurLifeStage }} | LifeStageDef (生命阶段) |
| ageTracker.Adult | {{ pawn.ageTracker.Adult }} | bool (成年) |
| ageTracker.Growth | {{ pawn.ageTracker.Growth }} | float (成长进度) |
| mindState.IsIdle | {{ pawn.mindState.IsIdle }} | bool (空闲) |
| mindState.InRoamingCooldown | {{ pawn.mindState.InRoamingCooldown }} | bool (漫游冷却中) |
| mindState.WillJoinColonyIfRescued | {{ pawn.mindState.WillJoinColonyIfRescued }} | bool (获救后加入) |
| mindState.AnythingPreventsJoiningColonyIfRescued | {{ pawn.mindState.AnythingPreventsJoiningColonyIfRescued }} | bool |
| mindState.CombatantRecently | {{ pawn.mindState.CombatantRecently }} | bool (近期参与战斗) |
| mindState.anyCloseHostilesRecently | {{ pawn.mindState.anyCloseHostilesRecently }} | bool (近期附近有敌对者) |
| needs.PrefersOutdoors | {{ pawn.needs.PrefersOutdoors }} | bool (偏好户外) |
| needs.PrefersIndoors | {{ pawn.needs.PrefersIndoors }} | bool (偏好室内) |
| needs.mood.CurLevelPercentage | {{ pawn.needs.mood.CurLevelPercentage }} | float (当前心情百分比) |
| needs.mood.MoodString | {{ pawn.needs.mood.MoodString }} | string (心情描述) |
| needs.food.CurLevelPercentage | {{ pawn.needs.food.CurLevelPercentage }} | float (食物/饥饿度) |
| needs.rest.CurLevelPercentage | {{ pawn.needs.rest.CurLevelPercentage }} | float (休息/疲劳度) |
| needs.joy.CurLevelPercentage | {{ pawn.needs.joy.CurLevelPercentage }} | float (娱乐/各种条) |
| needs.comfort.CurLevelPercentage | {{ pawn.needs.comfort.CurLevelPercentage }} | float (舒适度) |
| needs.energy.CurLevelPercentage | {{ pawn.needs.energy.CurLevelPercentage }} | float (能量-机械体) |
| story.Title | {{ pawn.story.Title }} | string (头衔) |
| story.TitleCap | {{ pawn.story.TitleCap }} | string |
| story.TitleShort | {{ pawn.story.TitleShort }} | string |
| story.TitleShortCap | {{ pawn.story.TitleShortCap }} | string |
| story.CaresAboutOthersAppearance | {{ pawn.story.CaresAboutOthersAppearance }} | bool (在意他人外貌) |
| guest.GuestStatus | {{ pawn.guest.GuestStatus }} | enum (访客状态) |
| guest.IsPrisoner | {{ pawn.guest.IsPrisoner }} | bool (是囚犯) |
| guest.IsSlave | {{ pawn.guest.IsSlave }} | bool (是奴隶) |
| guest.Resistance | {{ pawn.guest.Resistance }} | float (抵抗值) |
| guest.will | {{ pawn.guest.will }} | float (意志值) |
| guest.Recruitable | {{ pawn.guest.Recruitable }} | bool (可招募) |
| guest.EverEnslaved | {{ pawn.guest.EverEnslaved }} | bool (曾被奴役) |
| guest.Released | {{ pawn.guest.Released }} | bool (已释放) |
| relations.ChildrenCount | {{ pawn.relations.ChildrenCount }} | int (子女数量) |
| relations.RelatedToAnyoneOrAnyoneRelatedToMe | {{ pawn.relations.RelatedToAnyoneOrAnyoneRelatedToMe }} | bool (有亲属关系) |
| relations.IsTryRomanceOnCooldown | {{ pawn.relations.IsTryRomanceOnCooldown }} | bool (恋爱尝试冷却中) |
| ideo.Certainty | {{ pawn.ideo.Certainty }} | float (信仰确信度) |

# Map Scriban 字段（一级，可直接用于提示词）
注意：
- 一级字段使用 `map.<field>`，且必须能渲染为文本/数字/布尔值。
- 仅列出了用于提示词生成时安全的字段。

| 字段 | 示例 | 说明 |
| --- | --- | --- |
| weather | {{ map.weather }} | 特殊属性 |
| temperature | {{ map.temperature }} | 特殊属性 |
| IsPlayerHome | {{ map.IsPlayerHome }} | bool (是玩家据点) |
| IsStartingMap | {{ map.IsStartingMap }} | bool (是初始地图) |
| IsPocketMap | {{ map.IsPocketMap }} | bool (是口袋地图) |
| AgeInDays | {{ map.AgeInDays }} | float (地图存在天数) |
| PlayerWealthForStoryteller | {{ map.PlayerWealthForStoryteller }} | float (用于叙事者的财富值) |
| Tile | {{ map.Tile }} | PlanetTile (星球地块) |

# Map Scriban 字段（二级，可直接用于提示词）
注意：
- 二级字段使用 `map.<field>.<subfield>`。
- 仅限管理器内部使用或非文本类型的字段已排除。

| 字段 | 示例 | 说明 |
| --- | --- | --- |
| info.Size | {{ map.info.Size }} | IntVec3 (尺寸) |
| info.NumCells | {{ map.info.NumCells }} | int (格子总数) |
| info.isPocketMap | {{ map.info.isPocketMap }} | bool |
| TileInfo.temperature | {{ map.TileInfo.temperature }} | float (地块气温) |
| TileInfo.rainfall | {{ map.TileInfo.rainfall }} | float (地块降雨量) |
| TileInfo.elevation | {{ map.TileInfo.elevation }} | float (地块海拔) |
| TileInfo.hilliness | {{ map.TileInfo.hilliness }} | enum (地块地形起伏) |
| TileInfo.swampiness | {{ map.TileInfo.swampiness }} | float (沼泽程度) |
| TileInfo.pollution | {{ map.TileInfo.pollution }} | float (污染程度) |
| TileInfo.IsCoastal | {{ map.TileInfo.IsCoastal }} | bool (沿海) |
| TileInfo.AnimalDensity | {{ map.TileInfo.AnimalDensity }} | float (动物密度) |
| TileInfo.PlantDensityFactor | {{ map.TileInfo.PlantDensityFactor }} | float (植物密度系数) |
| TileInfo.MaxTemperature | {{ map.TileInfo.MaxTemperature }} | float (最高气温) |
| TileInfo.MinTemperature | {{ map.TileInfo.MinTemperature }} | float (最低气温) |
| Biome.label | {{ map.Biome.label }} | string (生物群落标签) |
| Biome.defName | {{ map.Biome.defName }} | string (生物群落定义名) |
| mapTemperature.OutdoorTemp | {{ map.mapTemperature.OutdoorTemp }} | float (室外温度) |
| mapTemperature.SeasonalTemp | {{ map.mapTemperature.SeasonalTemp }} | float (季节温度) |
| wealthWatcher.WealthTotal | {{ map.wealthWatcher.WealthTotal }} | float (总财富) |
| wealthWatcher.WealthItems | {{ map.wealthWatcher.WealthItems }} | float (物品财富) |
| wealthWatcher.WealthBuildings | {{ map.wealthWatcher.WealthBuildings }} | float (建筑财富) |
| wealthWatcher.WealthPawns | {{ map.wealthWatcher.WealthPawns }} | float (生物财富) |
| wealthWatcher.WealthFloorsOnly | {{ map.wealthWatcher.WealthFloorsOnly }} | float (仅地板财富) |
| wealthWatcher.HealthTotal | {{ map.wealthWatcher.HealthTotal }} | int (总生命值/健康度) |
| mapPawns.AllPawnsCount | {{ map.mapPawns.AllPawnsCount }} | int (所有 Pawn 数量) |
| mapPawns.AllPawnsUnspawnedCount | {{ map.mapPawns.AllPawnsUnspawnedCount }} | int (未生成的 Pawn 数量) |
| mapPawns.FreeColonistsCount | {{ map.mapPawns.FreeColonistsCount }} | int (自由殖民者数量) |
| mapPawns.PrisonersOfColonyCount | {{ map.mapPawns.PrisonersOfColonyCount }} | int (殖民地囚犯数量) |
| mapPawns.FreeColonistsAndPrisonersCount | {{ map.mapPawns.FreeColonistsAndPrisonersCount }} | int (自由殖民者和囚犯数量) |
| mapPawns.ColonistCount | {{ map.mapPawns.ColonistCount }} | int (殖民者数量) |

# Pawn Scriban 字段（二级，扩展：健康/技能/关系/思维）
注意：
- `pawn.health`、`pawn.skills` 和 `pawn.relations` 被同名的特殊属性键（magic keys）遮蔽（shadowed）了；除非修复映射关系，否则这些路径下的子字段可能无法访问。
- `pawn.mindState.*` 可以直接访问且使用安全。

| 字段 | 示例 | 说明 |
| --- | --- | --- |
| health.State | {{ pawn.health.State }} | enum (被父级遮蔽) |
| health.Downed | {{ pawn.health.Downed }} | bool (被父级遮蔽) |
| health.Dead | {{ pawn.health.Dead }} | bool (被父级遮蔽) |
| health.InPainShock | {{ pawn.health.InPainShock }} | bool (被父级遮蔽) |
| health.CanBleed | {{ pawn.health.CanBleed }} | bool (被父级遮蔽) |
| health.CanCrawl | {{ pawn.health.CanCrawl }} | bool (被父级遮蔽) |
| health.CanCrawlOrMove | {{ pawn.health.CanCrawlOrMove }} | bool (被父级遮蔽) |
| health.LethalDamageThreshold | {{ pawn.health.LethalDamageThreshold }} | float (被父级遮蔽) |
| health.summaryHealth.SummaryHealthPercent | {{ pawn.health.summaryHealth.SummaryHealthPercent }} | float (被父级遮蔽) |
| skills.PassionCount | {{ pawn.skills.PassionCount }} | int (被父级遮蔽) |
| skills.skills.Count | {{ pawn.skills.skills.Count }} | int (被父级遮蔽) |
| relations.ChildrenCount | {{ pawn.relations.ChildrenCount }} | int (被父级遮蔽) |
| relations.RelatedToAnyoneOrAnyoneRelatedToMe | {{ pawn.relations.RelatedToAnyoneOrAnyoneRelatedToMe }} | bool (被父级遮蔽) |
| relations.IsTryRomanceOnCooldown | {{ pawn.relations.IsTryRomanceOnCooldown }} | bool (被父级遮蔽) |
| mindState.Active | {{ pawn.mindState.Active }} | bool (活跃) |
| mindState.AvailableForGoodwillReward | {{ pawn.mindState.AvailableForGoodwillReward }} | bool (可作为好感度奖励) |
| mindState.IsIdle | {{ pawn.mindState.IsIdle }} | bool (空闲) |
| mindState.InRoamingCooldown | {{ pawn.mindState.InRoamingCooldown }} | bool (漫游冷却中) |
| mindState.CombatantRecently | {{ pawn.mindState.CombatantRecently }} | bool (近期参战) |
| mindState.MeleeThreatStillThreat | {{ pawn.mindState.MeleeThreatStillThreat }} | bool (近战威胁仍存在) |
| mindState.WillJoinColonyIfRescued | {{ pawn.mindState.WillJoinColonyIfRescued }} | bool (获救加入) |
| mindState.AnythingPreventsJoiningColonyIfRescued | {{ pawn.mindState.AnythingPreventsJoiningColonyIfRescued }} | bool (加入受阻) |
| mindState.anyCloseHostilesRecently | {{ pawn.mindState.anyCloseHostilesRecently }} | bool (近期附近有敌对) |
| mindState.lastJobTag | {{ pawn.mindState.lastJobTag }} | JobTag (上一个工作标签) |
| mindState.lastIngestTick | {{ pawn.mindState.lastIngestTick }} | int (上次进食 Tick) |
| mindState.lastHarmTick | {{ pawn.mindState.lastHarmTick }} | int (上次受伤 Tick) |
| mindState.exitMapAfterTick | {{ pawn.mindState.exitMapAfterTick }} | int (离图 Tick) |
| mindState.traderDismissed | {{ pawn.mindState.traderDismissed }} | bool (贸易商已解散) |
| mindState.hasQuest | {{ pawn.mindState.hasQuest }} | bool (有任务) |

# Pawn Scriban 字段（一级，补充：战斗/状态）
注意：
- 对战斗状态判断和提示词逻辑非常有用的直接 Pawn 属性。

| 字段 | 示例 | 说明 |
| --- | --- | --- |
| Drafted | {{ pawn.Drafted }} | bool (已征召) |
| Inspired | {{ pawn.Inspired }} | bool (有灵感/灵光一闪) |
| Crawling | {{ pawn.Crawling }} | bool (爬行中) |
| Flying | {{ pawn.Flying }} | bool (飞行中) |
| Swimming | {{ pawn.Swimming }} | bool (游泳中) |
| CanAttackWhileCrawling | {{ pawn.CanAttackWhileCrawling }} | bool (爬行时可攻击) |
| CanOpenDoors | {{ pawn.CanOpenDoors }} | bool (能开门) |
| CanOpenAnyDoor | {{ pawn.CanOpenAnyDoor }} | bool (能开任意门) |
| ShouldAvoidFences | {{ pawn.ShouldAvoidFences }} | bool (应避开栅栏) |
| FenceBlocked | {{ pawn.FenceBlocked }} | bool (被栅栏阻挡) |
| CanPassFences | {{ pawn.CanPassFences }} | bool (能翻越栅栏) |
| Roamer | {{ pawn.Roamer }} | bool (漫游者) |
| IsPrisonerOfColony | {{ pawn.IsPrisonerOfColony }} | bool (是殖民地囚犯) |
| IsSlaveOfColony | {{ pawn.IsSlaveOfColony }} | bool (是殖民地奴隶) |
| IsFreeNonSlaveColonist | {{ pawn.IsFreeNonSlaveColonist }} | bool (是自由非奴隶殖民者) |
| IsPlayerControlled | {{ pawn.IsPlayerControlled }} | bool (玩家控制) |
| IsColonyMechPlayerControlled | {{ pawn.IsColonyMechPlayerControlled }} | bool (玩家控制的殖民地机械体) |
| IsColonySubhumanPlayerControlled | {{ pawn.IsColonySubhumanPlayerControlled }} | bool (玩家控制的殖民地亚人) |
| IsAwokenCorpse | {{ pawn.IsAwokenCorpse }} | bool (苏醒的尸体) |
| IsDuplicate | {{ pawn.IsDuplicate }} | bool (是复制体) |
| IsEntity | {{ pawn.IsEntity }} | bool (是实体/收容物) |
| Deathresting | {{ pawn.Deathresting }} | bool (死眠中) |
| HasDeathRefusalOrResurrecting | {{ pawn.HasDeathRefusalOrResurrecting }} | bool (有拒死或正在复活) |
| HasPsylink | {{ pawn.HasPsylink }} | bool (有启灵/灵能) |
| HarmedByVacuum | {{ pawn.HarmedByVacuum }} | bool (受真空伤害) |
| ConcernedByVacuum | {{ pawn.ConcernedByVacuum }} | bool (受真空影响) |
| LastAttackTargetTick | {{ pawn.LastAttackTargetTick }} | int (上次攻击目标 Tick) |
| TicksPerMoveCardinal | {{ pawn.TicksPerMoveCardinal }} | float (直线移动每格耗时) |
| TicksPerMoveDiagonal | {{ pawn.TicksPerMoveDiagonal }} | float (斜向移动每格耗时) |

# Pawn Scriban 字段（二级，扩展：mindState 战斗）
注意：
- 这些属性可以通过 `pawn.mindState.*` 直接访问。

| 字段 | 示例 | 说明 |
| --- | --- | --- |
| mindState.Active | {{ pawn.mindState.Active }} | bool |
| mindState.AvailableForGoodwillReward | {{ pawn.mindState.AvailableForGoodwillReward }} | bool |
| mindState.IsIdle | {{ pawn.mindState.IsIdle }} | bool |
| mindState.InRoamingCooldown | {{ pawn.mindState.InRoamingCooldown }} | bool |
| mindState.CombatantRecently | {{ pawn.mindState.CombatantRecently }} | bool |
| mindState.MeleeThreatStillThreat | {{ pawn.mindState.MeleeThreatStillThreat }} | bool |
| mindState.WillJoinColonyIfRescued | {{ pawn.mindState.WillJoinColonyIfRescued }} | bool |
| mindState.AnythingPreventsJoiningColonyIfRescued | {{ pawn.mindState.AnythingPreventsJoiningColonyIfRescued }} | bool |
| mindState.anyCloseHostilesRecently | {{ pawn.mindState.anyCloseHostilesRecently }} | bool |
| mindState.lastJobTag | {{ pawn.mindState.lastJobTag }} | JobTag |
| mindState.lastIngestTick | {{ pawn.mindState.lastIngestTick }} | int |
| mindState.lastEngageTargetTick | {{ pawn.mindState.lastEngageTargetTick }} | int (上次交战 Tick) |
| mindState.lastAttackTargetTick | {{ pawn.mindState.lastAttackTargetTick }} | int (上次攻击 Tick) |
| mindState.lastMeleeThreatHarmTick | {{ pawn.mindState.lastMeleeThreatHarmTick }} | int (上次近战威胁造成伤害 Tick) |
| mindState.lastHarmTick | {{ pawn.mindState.lastHarmTick }} | int |
| mindState.lastCombatantTick | {{ pawn.mindState.lastCombatantTick }} | int (上次处于战斗状态 Tick) |
| mindState.canFleeIndividual | {{ pawn.mindState.canFleeIndividual }} | bool (能独自逃跑) |
| mindState.nextMoveOrderIsWait | {{ pawn.mindState.nextMoveOrderIsWait }} | bool (下个移动指令是等待) |
| mindState.nextMoveOrderIsCrawlBreak | {{ pawn.mindState.nextMoveOrderIsCrawlBreak }} | bool |
| mindState.wantsToTradeWithColony | {{ pawn.mindState.wantsToTradeWithColony }} | bool (想与殖民地交易) |
| mindState.traderDismissed | {{ pawn.mindState.traderDismissed }} | bool |
| mindState.hasQuest | {{ pawn.mindState.hasQuest }} | bool |
| mindState.lastTakeCombatEnhancingDrugTick | {{ pawn.mindState.lastTakeCombatEnhancingDrugTick }} | int (上次服用战斗增强药物 Tick) |
| mindState.lastTakeRecreationalDrugTick | {{ pawn.mindState.lastTakeRecreationalDrugTick }} | int (上次服用娱乐药物 Tick) |
| mindState.lastHumanMeatIngestedTick | {{ pawn.mindState.lastHumanMeatIngestedTick }} | int (上次吃人肉 Tick) |

# Pawn Scriban 字段（二级，扩展：健康/技能/关系）
注意：
- `pawn.health`、`pawn.skills` 和 `pawn.relations` 被 Scriban 中的特殊属性键遮蔽了；除非修复映射关系，否则子字段可能无法解析。

| 字段 | 示例 | 说明 |
| --- | --- | --- |
| health.State | {{ pawn.health.State }} | enum (被父级遮蔽) |
| health.Downed | {{ pawn.health.Downed }} | bool (被父级遮蔽) |
| health.Dead | {{ pawn.health.Dead }} | bool (被父级遮蔽) |
| health.InPainShock | {{ pawn.health.InPainShock }} | bool (被父级遮蔽) |
| health.CanBleed | {{ pawn.health.CanBleed }} | bool (被父级遮蔽) |
| health.CanCrawl | {{ pawn.health.CanCrawl }} | bool (被父级遮蔽) |
| health.CanCrawlOrMove | {{ pawn.health.CanCrawlOrMove }} | bool (被父级遮蔽) |
| health.LethalDamageThreshold | {{ pawn.health.LethalDamageThreshold }} | float (被父级遮蔽) |
| health.summaryHealth.SummaryHealthPercent | {{ pawn.health.summaryHealth.SummaryHealthPercent }} | float (被父级遮蔽) |
| health.hediffSet.PainTotal | {{ pawn.health.hediffSet.PainTotal }} | float (被父级遮蔽 - 疼痛总值) |
| health.hediffSet.BleedRateTotal | {{ pawn.health.hediffSet.BleedRateTotal }} | float (被父级遮蔽 - 出血率总值) |
| health.hediffSet.HasHead | {{ pawn.health.hediffSet.HasHead }} | bool (被父级遮蔽 - 有头) |
| health.hediffSet.PreventVacuumBurns | {{ pawn.health.hediffSet.PreventVacuumBurns }} | bool (被父级遮蔽 - 防真空烧伤) |
| health.hediffSet.HasRegeneration | {{ pawn.health.hediffSet.HasRegeneration }} | bool (被父级遮蔽 - 有再生能力) |
| health.hediffSet.RemoveRoamMtb | {{ pawn.health.hediffSet.RemoveRoamMtb }} | bool (被父级遮蔽) |
| health.hediffSet.HasPreventsDeath | {{ pawn.health.hediffSet.HasPreventsDeath }} | bool (被父级遮蔽 - 有防止死亡效果) |
| health.hediffSet.AnyHediffPreventsCrawling | {{ pawn.health.hediffSet.AnyHediffPreventsCrawling }} | bool (被父级遮蔽 - 有阻止爬行的 Hediff) |
| health.hediffSet.HungerRateFactor | {{ pawn.health.hediffSet.HungerRateFactor }} | float (被父级遮蔽 - 饥饿率系数) |
| health.hediffSet.RestFallFactor | {{ pawn.health.hediffSet.RestFallFactor }} | float (被父级遮蔽 - 疲劳率系数) |
| skills.PassionCount | {{ pawn.skills.PassionCount }} | int (被父级遮蔽 - 兴趣点总数) |
| skills.skills.Count | {{ pawn.skills.skills.Count }} | int (被父级遮蔽) |
| skills.skills[0].def.label | {{ pawn.skills.skills[0].def.label }} | string (被父级遮蔽 - 技能名) |
| skills.skills[0].Level | {{ pawn.skills.skills[0].Level }} | int (被父级遮蔽 - 等级) |
| skills.skills[0].LevelDescriptor | {{ pawn.skills.skills[0].LevelDescriptor }} | string (被父级遮蔽 - 等级描述) |
| skills.skills[0].passion | {{ pawn.skills.skills[0].passion }} | enum (被父级遮蔽 - 兴趣程度) |
| skills.skills[0].XpProgressPercent | {{ pawn.skills.skills[0].XpProgressPercent }} | float (被父级遮蔽 - 经验百分比) |
| skills.skills[0].LearningSaturatedToday | {{ pawn.skills.skills[0].LearningSaturatedToday }} | bool (被父级遮蔽 - 今日学习饱和) |
| skills.skills[0].TotallyDisabled | {{ pawn.skills.skills[0].TotallyDisabled }} | bool (被父级遮蔽 - 完全禁用) |
| skills.skills[0].PermanentlyDisabled | {{ pawn.skills.skills[0].PermanentlyDisabled }} | bool (被父级遮蔽 - 永久禁用) |
| skills.skills[0].Aptitude | {{ pawn.skills.skills[0].Aptitude }} | int (被父级遮蔽 - 资质/基因加成) |
| relations.DirectRelations.Count | {{ pawn.relations.DirectRelations.Count }} | int (被父级遮蔽 - 直接关系数量) |
| relations.VirtualRelations.Count | {{ pawn.relations.VirtualRelations.Count }} | int (被父级遮蔽 - 虚拟关系数量) |
| relations.ChildrenCount | {{ pawn.relations.ChildrenCount }} | int (被父级遮蔽 - 子女数量) |
| relations.RelatedToAnyoneOrAnyoneRelatedToMe | {{ pawn.relations.RelatedToAnyoneOrAnyoneRelatedToMe }} | bool (被父级遮蔽) |
| relations.IsTryRomanceOnCooldown | {{ pawn.relations.IsTryRomanceOnCooldown }} | bool (被父级遮蔽) |