================================================================================
  КАК СДЕЛАТЬ РАНДОМНЫЕ ТЕКСТУРЫ/ЦВЕТА МОБА ПРИ СПАВНЕ (по PackedAssets)
================================================================================

В игре нет отдельного поля "RandomTextureSet" для основной текстуры тела.
Рандом при спавне делается только через RandomAttachmentSets — по СЛОТАМ
(как уже сделано у Scorpion_Hollow для Head — рандомные черепа).

--------------------------------------------------------------------------------
СПОСОБ 1: РАЗНЫЕ ТЕКСТУРЫ (3–4 штуки) — Body как attachment
--------------------------------------------------------------------------------

ГДЕ:
  В JSON модели моба: Server\Models\Beast\Scorpion_Hollow.json (и других .json).

ЧТО ДЕЛАТЬ:

1) В BLOCKYMODEL (в редакторе):
   — Вынести "тело" (меш с основной текстурой) в отдельный ATTACHMENT.
   — То есть: основная модель без "шкуры", плюс одна нода (attachment point),
     к которой вешается меш тела. Имя ноды должно совпадать с именем слота
     в JSON (например "Body" или "Shell").
   — Если тело уже часть корневой геометрии — нужно пересобрать модель:
     корень без тела, тело — отдельный блок, привязанный к ноде Body.

2) В JSON МОДЕЛИ добавить или дополнить "RandomAttachmentSets". Пример слота
   для тела с 4 разными текстурами:

   "RandomAttachmentSets": {
     "Body": {
       "Variant1": {
         "Model": "NPC/Beast/Scorpion_Hollow/Models/Attachments/Body.blockymodel",
         "Texture": "NPC/Beast/Scorpion_Hollow/Models/Body_Texture_1.png",
         "Weight": 25
       },
       "Variant2": {
         "Model": "NPC/Beast/Scorpion_Hollow/Models/Attachments/Body.blockymodel",
         "Texture": "NPC/Beast/Scorpion_Hollow/Models/Body_Texture_2.png",
         "Weight": 25
       },
       "Variant3": {
         "Model": "NPC/Beast/Scorpion_Hollow/Models/Attachments/Body.blockymodel",
         "Texture": "NPC/Beast/Scorpion_Hollow/Models/Body_Texture_3.png",
         "Weight": 25
       },
       "Variant4": {
         "Model": "NPC/Beast/Scorpion_Hollow/Models/Attachments/Body.blockymodel",
         "Texture": "NPC/Beast/Scorpion_Hollow/Models/Body_Texture_4.png",
         "Weight": 25
       }
     },
     "Head": { ... }
   }

   — Имя слота ("Body") должно совпадать с именем ноды в blockymodel.
   — Weight — вес при выборе (равные = равный шанс).
   — Пути Model/Texture — под свои папки мода.

--------------------------------------------------------------------------------
СПОСОБ 2: ОДНА ТЕКСТУРА В СЕРОМ + ПАЛИТРА ЦВЕТОВ (GradientSet / GradientId)
--------------------------------------------------------------------------------

ГДЕ:
  — Текстура тела: одна в градациях серого (greyscale).
  — JSON модели: RandomAttachmentSets, слот с вариантами по цветам.
  — Палитры: PackedAssets\Cosmetics\CharacterCreator\GradientSets.json
    (или свой GradientSet в моде).

ЧТО ДЕЛАТЬ:

1) Сделать одну текстуру тела в градациях серого (белый/серый/чёрный).
   Игра подставит цвет по GradientId из выбранного GradientSet.

2) В JSON в RandomAttachmentSets добавить слот (имя = нода в модели для тела),
   в нём 3–4 варианта с одной и той же текстурой, но разным GradientId:

   "Body": {
     "Color_Blue": {
       "Model": "NPC/Beast/Scorpion_Hollow/Models/Attachments/Body.blockymodel",
       "Texture": "NPC/Beast/Scorpion_Hollow/Models/Body_Greyscale.png",
       "GradientSet": "Colored_Cotton",
       "GradientId": "Blue",
       "Weight": 25
     },
     "Color_Red": {
       "Model": "NPC/Beast/Scorpion_Hollow/Models/Attachments/Body.blockymodel",
       "Texture": "NPC/Beast/Scorpion_Hollow/Models/Body_Greyscale.png",
       "GradientSet": "Colored_Cotton",
       "GradientId": "Red",
       "Weight": 25
     },
     "Color_Green": { ... },
     "Color_Yellow": { ... }
   }

   — GradientSet — имя из GradientSets.json (например Colored_Cotton, Rotten_Fabric).
   — GradientId — конкретный цвет из этого набора (Blue, Red, Brown, Yellow и т.д.).
   — Тело снова должно быть вынесено в attachment в blockymodel (как в способе 1).

--------------------------------------------------------------------------------
ВАЖНО
--------------------------------------------------------------------------------

— Корневая "Texture" в JSON модели (в самом верху) — ОДНА на весь моб, её
  рандомно менять через конфиг нельзя. Рандом только через слоты в
  RandomAttachmentSets.

— Имя каждого слота в RandomAttachmentSets (например "Head", "Body") должно
  совпадать с именем ноды/attachment point в .blockymodel. Иначе слот не
  привяжется.

— У Scorpion_Hollow уже есть RandomAttachmentSets для "Head" (черепа). Чтобы
  добавить рандом тела — в модели нужна отдельная нода для тела (например
  "Body") и слот "Body" в JSON, как в примерах выше.

================================================================================
