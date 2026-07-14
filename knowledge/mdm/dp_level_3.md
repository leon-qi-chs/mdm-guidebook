# DP Level 3 数据映射规则

## 📋 概述

**数据来源:** [v5_pg_product - Product & Price Master Data Knowledge](https://pgitchina.visualstudio.com/Product%20&%20Price%20Master%20Data%20Knowledge)

**用途:** DP Level 3 是由 BU Demand Planner 提供，用于 GoldMine 和 Octopus 的数据使用。

**重要说明:** 每个 Category 的匹配逻辑都不同。

---

## 🔄 各 Category 映射逻辑

### Baby Care

**映射规则:**
```
DP Level 3 = CN Variant + CN Product Form
```

---

### Skin Care

**映射规则:**
- Skin Care 使用 **Hero Group** 作为 DP Level 3
- Hero Group 需要**手动维护**

---

### Hair Care

**映射规则:**
- DP L3 基于 **CN Variant**
- 具体映射关系见下表

#### Hair Care 映射表

| CN Variant | CN Variant Chinese | DP L3 EN |
|------------|-------------------|----------|
| RECONSTRUCTOR | 强韧修护 | Aussie |
| MIRACLE MOIST | 奇迹水润 | Aussie |
| AUSSOME VOL | 奇迹丰盈 | Aussie |
| Dry Shampoo | 免洗洗发喷雾 | Aussie |
| Anti Hair Fall Unisex | 青春防脱 | H&S Scalp X |
| Anti Hair Fall Male | 男士防脱 | H&S Scalp X |
| Hydrated Nourishment | 柔润滋养 | H&S Base |
| Perfuming | 香水 | H&S Base |
| Silky Soft | 丝质柔滑 | H&S Base |
| SPECIALLY DESIGNED FOR SEASONS | 季节专研 | H&S Base |
| Apple Fresh | 苹果清新 | H&S Base |
| Intensive Cleansing | 深层洁净 | H&S Base |
| Invigorating Refreshing | 怡神冰凉 | H&S Base |
| Itchy Pamering | 止痒呵护 | H&S Base |
| Ocean Lift | 海洋活力 | H&S Base |
| Oil Control | 清爽去油 | H&S Base |
| Perfume Charming | 幽香馥郁去屑 | H&S Base |
| Perfume Elegance | 恬雅清新去屑 | H&S Base |
| Protect Root | 护根防掉 | H&S Base |
| Radiant Black | 黑亮强韧 | H&S Base |
| Moisturizing | 组合补水系列 | H&S Base |
| Refresh | 组合净化系列 | H&S Base |
| Male Active Sport | 劲感酷爽 | H&S Base |
| Male Deep CLS Ntls | 劲感净透 | H&S Base |
| Male Mutiple Hydration | 劲感水润 | H&S Base |
| Male Oil Control | 劲感去油 | H&S Base |
| Male Strong Hair Root | 劲感强根 | H&S Base |
| Micellar | 微米系列 | H&S Supreme |
| Moisture | 水润去屑 | H&S Supreme |
| Smooth | 顺泽去屑 | H&S Supreme |
| Strength | 强韧去屑 | H&S Supreme |
| Moisture | 蜂蜜富养水润 | Hair Recipe |
| Smooth | 扁桃仁柔顺滋养 | Hair Recipe |
| Volume | 无花果清爽丰盈 | Hair Recipe |
| REPAIR | 苹果姜滋养修护 | Hair Recipe |
| BOOSTER OIL | 向日葵籽滋润护理 | Hair Recipe |
| BLUE GINGER | 头皮净活 | Herbal Essences |
| ARGAN OIL | 深层修护 | Herbal Essences |
| GRAPE FRUIT | 丰盈水润 | Herbal Essences |
| ROSEMARY | 养根韧发 | Herbal Essences |
| BIRCH BARK | 焕活赋能 | Herbal Essences |
| MIST PMC SPRAY | 晨露淡香 | Herbal Essences |
| ROSE WATER | 玫瑰 | Pantene COSMO |
| VIOLET WATER | 紫罗兰 | Pantene COSMO |
| COLOR MIRACLE COLD | 冷调 | Pantene PANTONE |
| COLOR MIRACLE WARM | 暖调 | Pantene PANTONE |
| COLOR MIRACLE NEUTRAL | 自然调 | Pantene PANTONE |
| COLOR MIRACLE | 显色提亮胶囊 | Pantene PANTONE |
| Weather Proof | 天气修护 | Pantene Weather Proof |
| 3MM | 3分钟奇迹 | Pantene 3MM |
| Black | 乌黑莹亮 | Pantene Base |
| Color Perm | 染烫修护 | Pantene Base |
| Milky Treatment | 乳液修护 | Pantene Base |
| Moisture Nourishing | 水润滋养 | Pantene Base |
| Nature | 植物精萃 | Pantene Base |
| Preserve | 强韧防掉发 | Pantene Base |
| Silky & Smooth | 丝质顺滑 | Pantene Base |
| Milky AD | 乳液修护去屑 | Pantene Base |
| Silky AD | 丝质顺滑去屑 | Pantene Base |
| Time Renewal | 时光修护 | Pantene Base |
| Ginza | 奢焕盈润 | Pantene Ginza |
| Moisturizing | 清润型 | Pantene Marilyn |
| Nourishing | 滋养型 | Pantene Marilyn |
| Pink | 赋能 | Pantene Marilyn |
| Light | 清润型 | Pantene Marilyn |
| DETOX GIN JAN | 排浊赋能 | Pantene Micellar |
| ENERGY | 头皮净透 | Pantene Micellar |
| HYDRA GIN JAN | 净油水润 | Pantene Micellar |
| HYDRATION | 芦荟水润亮泽 | Rejoice Non-Rinse |
| NOURISH | 人参滋养修护 | Rejoice Non-Rinse |
| LOVE IN PARIS | 香遇巴黎 | Rejoice Sapphire |
| MEDITERRANEAN BREEZE | 地中海微风 | Rejoice Sapphire |
| Collagen | 胶原蛋白组成因子 | Rejoice Essence Care Base |
| Essential Oil Nourishing smooth | 精油润养柔顺 | Rejoice Essence Care Base |
| Ginseng Nourishing Repair | 人参滋养修护 | Rejoice Essence Care Base |
| Moisture Anti-Dandruff | 滋润去屑 | Rejoice Essence Care Base |
| Natural Coconut | 植物椰油 | Rejoice Essence Care Base |
| Natural Silky & Soft | 丝质柔顺 | Rejoice Essence Care Base |
| OIL CONTROL&FRESH | 净油顺爽 | Rejoice Essence Care Base |
| Rich | 焗油护理 | Rejoice Essence Care Base |
| Straight & Shine | 垂顺亮泽 | Rejoice Essence Care Base |
| 3 in 1 | 3合1 | Rejoice Essence Care Base |
| Clean AD | 净爽去屑 | Rejoice Essence Care Base |
| Essential Oil AD | 精油去屑 | Rejoice Essence Care Base |
| Long hair | 维他命长发 | Rejoice Essence Care Base |
| Natural Fruit | 植物鲜果 | Rejoice Essence Care Base |
| Natural Herbal | 植物薄荷 | Rejoice Essence Care Base |
| Rich AD | 焗油护理去屑 | Rejoice Essence Care Base |
| Dual phase Oil | 双层精油 | Rejoice Essence Care Base |
| Long-Lasting Moisture | 兰花长效洁顺水润 | Rejoice Family Care Base |
| Smooth frh | 丝滑轻盈 | Rejoice Family Care Base |
| Long-Lasting Aloe Anti-itch | 芦荟长效止痒滋润 | Rejoice Family Care Base |
| Long-Lasting Fresh Anti-Dandruff | 兰花长效清爽去屑 | Rejoice Family Care Base |
| Long-Lasting Nourishing AD | 山茶长效柔顺去屑 | Rejoice Family Care Base |
| Long-Lasting Nourishing Nurturing | 杏仁长效柔顺滋养 | Rejoice Family Care Base |
| Long-Lating Oil Removal | 绿茶长效清爽去油 | Rejoice Family Care Base |
| Sunflower Seed Long Lasting Blackening& Moistruizing | 葵花籽精华长效黑亮滋润 | Rejoice Family Care Base |
| Flora Freya | 甜美花漾香氛 | Rejoice Freya |
| Ocean Freya | 海滩曼舞香氛 | Rejoice Freya |
| AIR&FRESH | 丰盈飘逸 | Rejoice Micellar Water |
| OIL CONTROL&FRESH | 净油顺爽 | Rejoice Micellar Water |
| Smooth AD | 净油去屑 | Rejoice Micellar Water |
| Styling | 定型产品 | VS styling |
| Freezing Hold Spray | 速挺定型喷雾 | VS styling |
| Steel Molding Gel | 钢铁劲强啫喱 | VS styling |
| Raise the root spray | 秀发蓬松水 | VS styling |
| Matt Molding Clay | 逆重哑光发泥 | VS styling |
| Texturized Shaping Wax | 凌乱动感发蜡 | VS styling |
| Lock 'N Load Spray | 强力定型喷雾 | VS styling |
| Firm 'N flexible Spray | 肆意定型喷雾 | VS styling |
| Hydrating Essence Water | 焕彩柔顺精华水 | VS styling |
| Molding Gel | 塑型啫喱 | VS styling |
| Shaping Gel Spray | 强力持久造型啫喱喷雾 | VS styling |
| Shaping Mousse | 强力持久造型摩丝 | VS styling |
| Curl Favorite | 盈卷弹力乳 | VS styling |
| Male Style Ready | 净化打底男士 | VS Base |
| Male Hydra Balance | 水润平衡男士 | VS Base |
| Male Make Pefresh Awasking | 清爽劲醒男士 | VS Base |
| Male AD | 男士洁净去屑 | VS Base |
| COLOR LOCK PURPLE | 漂染后锁色 - 紫色 | VS Color |
| COLOR LOCK GREY | 漂染后锁色 - 灰色 | VS Color |
| Styling Remover | 造型卸妆 | VS Base |
| Curl Strenthen | 弹韧卷发 | VS Base |
| Color Keeper | 亮泽护色 | VS Base |
| Textirozed & Weighted | 垂坠质感 | VS Base |
| Moisture Treatment Repair | 修护水养 | VS Base |
| Vivid Color Care | 炫亮彩护 | VS Base |
| Light&Soft Smoothness | 清盈顺柔 | VS Base |
| Illuminating Moisture | 光感莹润 | VS Base |
| Captivating Curl Care | 盈卷修润 | VS Base |
| Repair Foam | 臻养云吻泡沫 | VS Base |
| Light & Soft Foam | 轻润云吻泡沫 | VS Base |
| Moisturizing AD | 水润去屑 | VS Base |
| Scalp Purify | 头皮净澈裸感 | VS Nilsil |
| Fortify Care | 养根韧发裸感 | VS Nilsil |
| Repair Nude | 臻养裸感 | VS Nilsil |
| LIGHT SOFT NUDE | 轻润裸感 | VS Nilsil |

---

## 📝 使用说明

1. **Baby Care**: 直接使用 CN Variant + CN Product Form 组合
2. **Skin Care**: 使用 Hero Group（需手动维护）
3. **Hair Care**: 根据上述映射表，通过 CN Variant 查找对应的 DP L3 EN

---

**文档版本:** v1.0  
**最后更新:** 2025-01-08
