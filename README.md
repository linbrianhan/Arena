<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>暗區聯絡人物品兌換搜尋</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            padding: 0;
            background-color: #f4f4f4;
        }
        h1{
            font-size: 24px;
            text-align: center;
        }
        #searchResult, #materialResult {
            margin-top: 20px;
        }
        .item {
            margin-bottom: 10px;
            background: white;
            padding: 10px;
            border-radius: 5px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }
        .button {
            padding: 5px 15px;
            cursor: pointer;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 5px;
            width: 100%;
            font-size: 16px;
            margin-top: 10px;
            transition: background-color 0.3s;
        }
        .button:hover {
            background-color: #0056b3;
        }
        .description {
            color: #555;
            font-style: italic;
            margin-top: 10px;
        }
        input[type="text"] {
            width: 100%;
            padding: 10px;
            font-size: 16px;
            border-radius: 5px;
            border: 1px solid #ddd;
            box-sizing: border-box;
        }
        @media (max-width: 768px) {
            h1 {
                font-size: 20px;
            }
            .button {
                padding: 12px;
                font-size: 18px;
            }
            input[type="text"] {
                padding: 12px;
                font-size: 18px;
            }
        }
    </style>
</head>
<body>

    <h1>暗區聯絡人兌換搜尋</h1>

    <div>
        <h2>搜尋物品名稱</h2>
        <input type="text" id="searchInput" placeholder="輸入物品名稱">
        <button class="button" onclick="searchItem()">搜尋</button>  
        <div id="searchResult"></div> <!-- 獨立顯示搜尋結果 -->
    </div>

    <div>
        <h2>根據材料搜尋物品</h2>
        <input type="text" id="materialInput" placeholder="輸入材料名稱">
        <button class="button" onclick="searchByMaterials()">搜尋</button>   
        <div id="materialResult"></div> <!-- 獨立顯示材料搜尋結果 -->
    </div>

    <script>
        // 模擬合成公式資料
        const items = [
            {
                "name": "T85微型衝鋒槍",
                "materials": [
                    {"name": "棉布", "quantity": 1},
                    {"name": "安全繩", "quantity": 1}
                ],
                "description": "兌換人:喬爾.加里森"
            },
            {
                "name": "MAC 10微型衝鋒槍",
                "materials": [
                    {"name": "老式收音機", "quantity": 2},
                    {"name": "電池", "quantity": 3}
                ],
                "description": "兌換人:喬爾.加里森"
            },
            {
                "name": "Vector45微型衝鋒槍",
                "materials": [
                    {"name": "PSU電源", "quantity": 1},
                    {"name": "工具箱", "quantity": 3}
                ],
                "description": "兌換人:喬爾.加里森"
            },
            {
                "name": "S12霰彈槍",
                "materials": [
                    {"name": "節能燈", "quantity": 3},
                    {"name": "軍用羅盤", "quantity": 4}
                ],
                "description": "兌換人:喬爾.加里森"
            },
            {
                "name": "USAS12霰彈槍",
                "materials": [
                    {"name": "鼓風機", "quantity": 3},
                    {"name": "相框", "quantity": 2}
                ],
                "description": "兌換人:喬爾.加里森"
            },
            {
                "name": "T951突擊步槍",
                "materials": [
                    {"name": "銀質手鍊", "quantity": 2},
                    {"name": "完好螢幕", "quantity": 1}
                ],
                "description": "兌換人:喬爾.加里森"
            },
            {
                "name": "T03突擊步槍",
                "materials": [
                    {"name": "香水", "quantity": 1},
                    {"name": "座鐘", "quantity": 3},
                    {"name": "懷錶", "quantity": 3}
                ],
                "description": "兌換人:喬爾.加里森"
            },
            {
                "name": ".45口徑AP子彈 3級",
                "materials": [
                    {"name": "陶瓷杯", "quantity": 3},
                    {"name": "刮鬍刀", "quantity": 3}
                ],
                "description": "兌換人:喬爾.加里森"
            },
            {
                "name": "5.8x42mm DBP87子彈 3級",
                "materials": [
                    {"name": "燈泡", "quantity": 2},
                    {"name": "消毒水", "quantity": 2}
                ],
                "description": "兌換人:喬爾.加里森"
            },
            {
                "name": "簡易紅點瞄準鏡",
                "materials": [
                    {"name": "撲克牌", "quantity": 2},
                    {"name": "卡莫納鴞毛絨玩具", "quantity": 2}
                ],
                "description": "兌換人:喬爾.加里森"
            },
            {
                "name": "MP 133 710mm槍管",
                "materials": [
                    {"name": "搪瓷杯", "quantity": 2},
                    {"name": "肥皂", "quantity": 2}
                ],
                "description": "兌換人:喬爾.加里森"
            },
            {
                "name": "925快速急救包",
                "materials": [
                    {"name": "肥皂", "quantity": 2},
                    {"name": "生理食鹽水", "quantity": 1}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "E3軍用醫療包",
                "materials": [
                    {"name": "雙氧水", "quantity": 1},
                    {"name": "醫用海綿", "quantity": 2}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "1000戰地醫療包",
                "materials": [
                    {"name": "牙膏", "quantity": 5},
                    {"name": "針管", "quantity": 1}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "提神藥水",
                "materials": [
                    {"name": "牙膏", "quantity": 2},
                    {"name": "生理食鹽水", "quantity": 1}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "濃縮鎮痛劑",
                "materials": [
                    {"name": "醫用海綿", "quantity": 1},
                    {"name": "牙膏", "quantity": 2}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "快速止痛藥",
                "materials": [
                    {"name": "車用尿素", "quantity": 1},
                    {"name": "BP機", "quantity": 1}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "AP鎮痛片",
                "materials": [
                    {"name": "紫外線燈", "quantity": 1},
                    {"name": "醫用海綿", "quantity": 5}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "標準軍用手術包",
                "materials": [
                    {"name": "針管", "quantity": 2},
                    {"name": "醫用剪刀", "quantity": 1}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "耐力注射劑",
                "materials": [
                    {"name": "雙氧水", "quantity": 1},
                    {"name": "肥皂", "quantity": 2}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "肌肉注射劑",
                "materials": [
                    {"name": "牙膏", "quantity": 1},
                    {"name": "針管", "quantity": 1}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "再生注射劑",
                "materials": [
                    {"name": "漂白劑", "quantity": 2},
                    {"name": "葡萄糖", "quantity": 1}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "民宅鑰匙",
                "materials": [
                    {"name": "打火機", "quantity": 4},
                    {"name": "金屬火機", "quantity": 4},
                    {"name": "錐子", "quantity": 3}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "藍色火藥",
                "materials": [
                    {"name": "疏通劑", "quantity": 1},
                    {"name": "牙膏", "quantity": 1},
                    {"name": "肥皂", "quantity": 2}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "綠色火藥",
                "materials": [
                    {"name": "藍色火藥", "quantity": 2}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "注射用葡萄糖",
                "materials": [
                    {"name": "雙氧水", "quantity": 1},
                    {"name": "軍用急救包", "quantity": 2},
                    {"name": "生理食鹽水", "quantity": 1}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "醫用雜物盒",
                "materials": [
                    {"name": "醫用海綿", "quantity": 2},
                    {"name": "醫用剪刀", "quantity": 1}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "工具套組",
                "materials": [
                    {"name": "螺絲刀", "quantity": 2},
                    {"name": "老虎鉗", "quantity": 2},
                    {"name": "粗枝剪", "quantity": 2},
                    {"name": "多功能工具", "quantity": 1}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "手機",
                "materials": [
                    {"name": "多功能工具", "quantity": 1},
                    {"name": "手機電池", "quantity": 1},
                    {"name": "損毀的手機", "quantity": 1}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "紅色火藥",
                "materials": [
                    {"name": "綠色火藥", "quantity": 2}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "藥物霧化吸入器",
                "materials": [
                    {"name": "醫用海綿", "quantity": 2},
                    {"name": "注射用葡萄糖", "quantity": 1},
                    {"name": "生理食鹽水", "quantity": 2}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "丙烷罐",
                "materials": [
                    {"name": "煤油燈", "quantity": 1},
                    {"name": "過濾罐", "quantity": 2},
                    {"name": "壓力計", "quantity": 1}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "體溫槍",
                "materials": [
                    {"name": "醫用雜物盒", "quantity": 2},
                    {"name": "消毒水", "quantity": 2},
                    {"name": "針管", "quantity": 1}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "完好的螢幕",
                "materials": [
                    {"name": "攝影機", "quantity": 2},
                    {"name": "破損的螢幕", "quantity": 2}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "燃料桶",
                "materials": [
                    {"name": "油桶", "quantity": 2},
                    {"name": "電鑽", "quantity": 1}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "鈦合金板",
                "materials": [
                    {"name": "鋼管", "quantity": 2},
                    {"name": "丙烷罐", "quantity": 1}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "擴充倉庫啟動卡",
                "materials": [
                    {"name": "唱片機", "quantity": 1},
                    {"name": "保養良好如全新的軍刀", "quantity": 10},
                    {"name": "手工製金幣", "quantity": 5},
                    {"name": "宮廷銀器", "quantity": 1}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "普通的鑰匙鏈(2*3)",
                "materials": [
                    {"name": "金杯", "quantity": 3},
                    {"name": "歷戰的徽章", "quantity": 1},
                    {"name": "保養良好如全新的軍刀", "quantity": 1},
                    {"name": "手工製金幣", "quantity": 3}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "擴充倉庫啟動卡",
                "materials": [
                    {"name": "黃金魔術方塊", "quantity": 10},
                    {"name": "歷戰的徽章", "quantity": 10},
                    {"name": "保養良好如全新的軍刀", "quantity": 10},
                    {"name": "手工製金幣", "quantity": 10}
                ],
                "description": "兌換人:艾薇塔"
            },
            {
                "name": "SH18軍用頭盔 LV4",
                "materials": [
                    {"name": "DVD", "quantity": 2},
                    {"name": "保險", "quantity": 3}
                ],
                "description": "兌換人:迪克.文森"
            },
            {
                "name": "SH40軍用頭盔 LV4",
                "materials": [
                    {"name": "手機", "quantity": 2},
                    {"name": "電容", "quantity": 3}
                ],
                "description": "兌換人:迪克.文森"
            },
            {
                "name": "H-Tac特勤防彈衣 LV3",
                "materials": [
                    {"name": "防水膠帶", "quantity": 2},
                    {"name": "D型電池", "quantity": 2},
                    {"name": "T行插頭", "quantity": 2}
                ],
                "description": "兌換人:迪克.文森"
            },
            {
                "name": "6B13防彈衣 LV4",
                "materials": [
                    {"name": "呢絨布", "quantity": 2},
                    {"name": "便攜酒精爐", "quantity": 1}
                ],
                "description": "兌換人:迪克.文森"
            },
            {
                "name": "926複合防彈衣 LV5",
                "materials": [
                    {"name": "半導體晶片", "quantity": 2},
                    {"name": "天線", "quantity": 1}
                ],
                "description": "兌換人:迪克.文森"
            },
            {
                "name": "RAP簡易彈掛",
                "materials": [
                    {"name": "老虎鉗", "quantity": 1},
                    {"name": "文件", "quantity": 1}
                ],
                "description": "兌換人:迪克.文森"
            },
            {
                "name": "B4戰術彈掛",
                "materials": [
                    {"name": "節能燈", "quantity": 1},
                    {"name": "破損的螢幕", "quantity": 2}
                ],
                "description": "兌換人:迪克.文森"
            },
            {
                "name": "運動背包",
                "materials": [
                    {"name": "疏通劑", "quantity": 2},
                    {"name": "消毒水", "quantity": 2}
                ],
                "description": "兌換人:迪克.文森"
            },
            {
                "name": "帆布野營背包",
                "materials": [
                    {"name": "螺絲釘", "quantity": 3},
                    {"name": "有機玻璃", "quantity": 2}
                ],
                "description": "兌換人:迪克.文森"
            },
            {
                "name": "戶外旅行背包",
                "materials": [
                    {"name": "表面清潔劑", "quantity": 3},
                    {"name": "麻布", "quantity": 3}
                ],
                "description": "兌換人:迪克.文森"
            },
            {
                "name": "RUSH戰術背包",
                "materials": [
                    {"name": "BP機", "quantity": 2},
                    {"name": "橡膠墊", "quantity": 2}
                ],
                "description": "兌換人:迪克.文森"
            },
            {
                "name": "牛皮背包",
                "materials": [
                    {"name": "打火機", "quantity": 3},
                    {"name": "老式收音機", "quantity": 2}
                ],
                "description": "兌換人:迪克.文森"
            },
            {
                "name": "大型登山背包",
                "materials": [
                    {"name": "移動電源", "quantity": 2},
                    {"name": "郵票", "quantity": 2}
                ],
                "description": "兌換人:迪克.文森"
            },
            {
                "name": "Med野戰背包",
                "materials": [
                    {"name": "管鉗", "quantity": 3},
                    {"name": "文件", "quantity": 2}
                ],
                "description": "兌換人:迪克.文森"
            },
            {
                "name": "老式行軍背包",
                "materials": [
                    {"name": "發條小汽車", "quantity": 1},
                    {"name": "傘繩", "quantity": 1}
                ],
                "description": "兌換人:迪克.文森"
            },
            {
                "name": "查普曼軍用背包",
                "materials": [
                    {"name": "軍用水壺", "quantity": 3},
                    {"name": "測距望遠鏡", "quantity": 1}
                ],
                "description": "兌換人:迪克.文森"
            },
            {
                "name": "XA4戰術背包",
                "materials": [
                    {"name": "電池", "quantity": 4},
                    {"name": "小袋肥料", "quantity": 1}
                ],
                "description": "兌換人:迪克.文森"
            },
            {
                "name": "AMP7突擊背包",
                "materials": [
                    {"name": "消毒水", "quantity": 1},
                    {"name": "油桶", "quantity": 1},
                    {"name": "水泥", "quantity": 1}
                ],
                "description": "兌換人:迪克.文森"
            },
            {
                "name": "莫辛納甘栓動步槍",
                "materials": [
                    {"name": "螺絲釘", "quantity": 6},
                    {"name": "傘繩", "quantity": 2}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "AKM突擊步槍",
                "materials": [
                    {"name": "膠水", "quantity": 3},
                    {"name": "損毀的手機", "quantity": 3}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "AK 102突擊步槍",
                "materials": [
                    {"name": "矽膠管", "quantity": 3},
                    {"name": "破損的螢幕", "quantity": 4},
                    {"name": "磁鐵", "quantity": 2}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "AK 74N突擊步槍",
                "materials": [
                    {"name": "矽膠管", "quantity": 2},
                    {"name": "車用尿素", "quantity": 3},
                    {"name": "洗髮精", "quantity": 3}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "ACE31突擊步槍",
                "materials": [
                    {"name": "機油壺", "quantity": 2},
                    {"name": "手持補光燈", "quantity": 1}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "SVDS狙擊步槍",
                "materials": [
                    {"name": "丙烷罐", "quantity": 2},
                    {"name": "GPU", "quantity": 2}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "VSS射手步槍",
                "materials": [
                    {"name": "電鑽", "quantity": 3},
                    {"name": "照相機", "quantity": 1}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "RPK16輕機槍",
                "materials": [
                    {"name": "火星塞", "quantity": 4},
                    {"name": "便攜式酒精爐", "quantity": 4}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "AEK突擊步槍",
                "materials": [
                    {"name": "軟硬碟", "quantity": 3},
                    {"name": "隔音棉", "quantity": 1}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "AKM系列50發相容彈鼓",
                "materials": [
                    {"name": "撲克牌", "quantity": 3},
                    {"name": "文件", "quantity": 1}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "7.62x39mm PS子彈 LV3",
                "materials": [
                    {"name": "藍色火藥", "quantity": 1},
                    {"name": "電線", "quantity": 2}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "5.45x39mm PP子彈 LV3",
                "materials": [
                    {"name": "撲克牌", "quantity": 3},
                    {"name": "文件", "quantity": 1}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "7.62x54 T46M子彈 LV3",
                "materials": [
                    {"name": "磁鐵", "quantity": 2},
                    {"name": "CPU風扇", "quantity": 1}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "莫辛栓動步槍消音器",
                "materials": [
                    {"name": "靜電膠帶", "quantity": 3},
                    {"name": "腕錶", "quantity": 1}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "46多口徑消音器",
                "materials": [
                    {"name": "撲克牌", "quantity": 2},
                    {"name": "損毀的", "quantity": 2}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "AKM精準消焰器",
                "materials": [
                    {"name": "電纜", "quantity": 2},
                    {"name": "電線", "quantity": 2}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "AK系列5.45x39口徑消音器",
                "materials": [
                    {"name": "CPU風扇", "quantity": 2},
                    {"name": "磁片", "quantity": 1}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "AK系列FAB後托",
                "materials": [
                    {"name": "節能登", "quantity": 1},
                    {"name": "水壺", "quantity": 3}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "81式槍托",
                "materials": [
                    {"name": "螺絲刀,", "quantity": 2},
                    {"name": "老式收音機", "quantity": 3}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "莫辛瞄具擴充導軌",
                "materials": [
                    {"name": "電池", "quantity": 1},
                    {"name": "金屬火機", "quantity": 1}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "AK系列CAA聚合前護木",
                "materials": [
                    {"name": "螺母", "quantity": 1},
                    {"name": "訊號接收器", "quantity": 1}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "AK便捷改裝護木",
                "materials": [
                    {"name": "綠色火藥", "quantity": 1},
                    {"name": "醫用雜物盒", "quantity": 2}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "AK特製導軌防塵蓋",
                "materials": [
                    {"name": "膠帶", "quantity": 1},
                    {"name": "電線", "quantity": 2}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "AK適用導軌直列防塵蓋",
                "materials": [
                    {"name": "打火機", "quantity": 2},
                    {"name": "搪瓷杯", "quantity": 1}
                ],
                "description": "兌換人:弗拉德倫"
            },
            {
                "name": "MP5微型衝鋒槍",
                "materials": [
                    {"name": "螺絲刀", "quantity": 1},
                    {"name": "武器零件", "quantity": 3}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "Vector9衝鋒槍",
                "materials": [
                    {"name": "鋼管", "quantity": 4},
                    {"name": "黃銅管", "quantity": 1}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "Mini14卡賓槍",
                "materials": [
                    {"name": "手電筒", "quantity": 1},
                    {"name": "水壺", "quantity": 2}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "M16卡賓槍",
                "materials": [
                    {"name": "平衡儀", "quantity": 1},
                    {"name": "快門線", "quantity": 3}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "G18C手槍",
                "materials": [
                    {"name": "銀徽章", "quantity": 2},
                    {"name": "傘繩", "quantity": 2}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "MP9微型衝鋒槍",
                "materials": [
                    {"name": "移動電源", "quantity": 3},
                    {"name": "鼓風機", "quantity": 1}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "M4A1突擊步槍",
                "materials": [
                    {"name": "耐衝擊硬膠", "quantity": 3},
                    {"name": "磁帶隨身聽", "quantity": 1}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "SCAR-L突擊步槍",
                "materials": [
                    {"name": "固態硬碟", "quantity": 1},
                    {"name": "金絲邊眼鏡", "quantity": 1}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "MPX微型衝鋒槍",
                "materials": [
                    {"name": "工具套組", "quantity": 3},
                    {"name": "硬殼筆記本", "quantity": 1}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "SJ16栓動步槍",
                "materials": [
                    {"name": "防彈膠", "quantity": 3},
                    {"name": "PSU電源", "quantity": 5}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "沙漠之鷹手槍",
                "materials": [
                    {"name": "膠帶", "quantity": 3},
                    {"name": "防水膠帶", "quantity": 4},
                    {"name": "削鉛筆機", "quantity": 4}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "H416突擊步槍",
                "materials": [
                    {"name": "手機電池", "quantity": 3},
                    {"name": "電影膠片", "quantity": 3}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "DT MDR突擊步槍",
                "materials": [
                    {"name": "安全錘", "quantity": 4},
                    {"name": "炮隊鏡", "quantity": 4}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "F2000突擊步槍",
                "materials": [
                    {"name": "PSU電源", "quantity": 3},
                    {"name": "萬能表", "quantity": 4},
                    {"name": "磁片", "quantity": 3}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "MP5 50發彈匣",
                "materials": [
                    {"name": "移動電源", "quantity": 3}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "MPX 41發加長擴充彈匣",
                "materials": [
                    {"name": "筆記本", "quantity": 4},
                    {"name": "螺絲釘", "quantity": 2}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "MPX 50發聚合彈鼓",
                "materials": [
                    {"name": "露營燈", "quantity": 1},
                    {"name": "DVD", "quantity": 2}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "SJ16 5發彈匣",
                "materials": [
                    {"name": "管鉗", "quantity": 3},
                    {"name": "T型插頭", "quantity": 2}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "AR槍械系列通用60發聚合彈鼓",
                "materials": [
                    {"name": "工具套組", "quantity": 1},
                    {"name": "紀念硬幣", "quantity": 1}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "G系手槍 33發加長彈匣",
                "materials": [
                    {"name": "漂白劑", "quantity": 2},
                    {"name": "電路板", "quantity": 1}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "5.56x45mm M855子彈 LV3",
                "materials": [
                    {"name": "藍色火藥", "quantity": 2},
                    {"name": "螺絲釘", "quantity": 4}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "9x19mm AP6.3子彈 LV3",
                "materials": [
                    {"name": "黃銅管", "quantity": 2},
                    {"name": "電線", "quantity": 2}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "4倍瞄準鏡",
                "materials": [
                    {"name": "懷表", "quantity": 2},
                    {"name": "炮隊鏡", "quantity": 1}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "3.5倍瞄準鏡",
                "materials": [
                    {"name": "老虎鉗", "quantity": 1},
                    {"name": "測距望遠鏡", "quantity": 3}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "5.56x45口徑消音器",
                "materials": [
                    {"name": "遊戲卡帶", "quantity": 2},
                    {"name": "除鏽劑", "quantity": 1}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "MP5戰術槍托",
                "materials": [
                    {"name": "筆記本", "quantity": 3},
                    {"name": "相框", "quantity": 1}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "AR緩衝槍托",
                "materials": [
                    {"name": "高效能手電筒", "quantity": 2},
                    {"name": "過濾罐", "quantity": 2}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "2.5英吋模組鎖導軌",
                "materials": [
                    {"name": "螺母", "quantity": 2},
                    {"name": "膠帶", "quantity": 2}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "2英吋模組鎖導軌",
                "materials": [
                    {"name": "錐子", "quantity": 2},
                    {"name": "牙膏", "quantity": 2}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "4.1英吋模組鎖導軌",
                "materials": [
                    {"name": "金屬火機", "quantity": 2},
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "AR特製通用護木",
                "materials": [
                    {"name": "多功能工具", "quantity": 2},
                    {"name": "訊號干擾器", "quantity": 2}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "AR系列卡賓槍長度護木",
                "materials": [
                    {"name": "老虎鉗", "quantity": 2},
                    {"name": "攝影機", "quantity": 3}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "MP5SD聚合護木",
                "materials": [
                    {"name": "錐子", "quantity": 1},
                    {"name": "氣體分析儀", "quantity": 1}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "MPX10.5英吋模組鎖護木",
                "materials": [
                    {"name": "手寫板", "quantity": 2},
                    {"name": "訊號干擾器", "quantity": 2}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "MPX14英吋模組鎖護木",
                "materials": [
                    {"name": "懷表", "quantity": 1},
                    {"name": "水壺", "quantity": 3}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "MP5高性能SD上機匣",
                "materials": [
                    {"name": "筆記本", "quantity": 3},
                    {"name": "保險絲", "quantity": 2}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "MPX標準槍管",
                "materials": [
                    {"name": "雙氧水", "quantity": 2},
                    {"name": "BP機", "quantity": 2}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "MPX加長槍管",
                "materials": [
                    {"name": "電纜", "quantity": 2},
                    {"name": "發條小汽車", "quantity": 1}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "H416A5通用標準槍管",
                "materials": [
                    {"name": "記憶體", "quantity": 1},
                    {"name": "D型電池", "quantity": 3}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "H416A5通用中型槍管",
                "materials": [
                    {"name": "表面清潔劑", "quantity": 2},
                    {"name": "傘繩", "quantity": 1}
                ],
                "description": "兌換人:湯瑪斯.愛德華"
            },
            {
                "name": "BM59卡賓槍",
                "materials": [
                    {"name": "高效能手電筒", "quantity": 1},
                    {"name": "電源線", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "M14卡賓槍",
                "materials": [
                    {"name": "螺母", "quantity": 4},
                    {"name": "訊號接收器", "quantity": 2}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "FAL突擊步槍",
                "materials": [
                    {"name": "金絲邊眼鏡", "quantity": 2},
                    {"name": "文件", "quantity": 2}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "M110射手步槍",
                "materials": [
                    {"name": "氣密性檢測儀", "quantity": 3},
                    {"name": "水泥", "quantity": 2}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "M24栓動步槍",
                "materials": [
                    {"name": "老式散熱器", "quantity": 3},
                    {"name": "疏通工具組", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "P90微型衝鋒槍",
                "materials": [
                    {"name": "手機", "quantity": 3},
                    {"name": "鈦合金板", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "AR57突擊步槍",
                "materials": [
                    {"name": "油桶", "quantity": 3},
                    {"name": "助聽器", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "BM59 10發彈匣",
                "materials": [
                    {"name": "蠟燭", "quantity": 1},
                    {"name": "捲尺", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "M110 20發快速彈匣",
                "materials": [
                    {"name": "粗枝剪", "quantity": 2},
                    {"name": "麻布", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "P90 30發聚合彈匣",
                "materials": [
                    {"name": "粗枝剪", "quantity": 2},
                    {"name": "BP機", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "FAL 50發彈鼓",
                "materials": [
                    {"name": "卡莫納鴞毛絨玩具", "quantity": 2},
                    {"name": "蠟燭", "quantity": 2}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "7.62x51mm BPZ子彈 LV3",
                "materials": [
                    {"name": "燈泡", "quantity": 3},
                    {"name": "整流器", "quantity": 2}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "5.7x28mm SS197SR子彈 LV3",
                "materials": [
                    {"name": "波紋管", "quantity": 2},
                    {"name": "郵票", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "IND200頭盔 LV4",
                "materials": [
                    {"name": "有機玻璃", "quantity": 5},
                    {"name": "無限對講機", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "03重型戰術頭盔 LV5",
                "materials": [
                    {"name": "體溫槍", "quantity": 3},
                    {"name": "胰島素泵", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "T7熱成像儀",
                "materials": [
                    {"name": "工具箱", "quantity": 3},
                    {"name": "安全錘", "quantity": 3},
                    {"name": "熱成像模組", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "IND404型防彈衣 L4",
                "materials": [
                    {"name": "老虎鉗", "quantity": 3},
                    {"name": "無線對講機", "quantity": 2}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "BT6重型防彈衣 LV5",
                "materials": [
                    {"name": "卡莫納鴞毛絨玩具", "quantity": 2},
                    {"name": "炮隊鏡", "quantity": 4},
                    {"name": "裝飾手杖", "quantity": 4}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "6B5彈掛護甲 L3",
                "materials": [
                    {"name": "波紋管", "quantity": 1},
                    {"name": "過濾罐", "quantity": 2}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "哨兵305胸掛護甲 L4",
                "materials": [
                    {"name": "耐衝擊硬膠", "quantity": 1},
                    {"name": "化纖", "quantity": 3}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "TM1胸掛護甲 L4",
                "materials": [
                    {"name": "丙烷罐", "quantity": 2},
                    {"name": "硬殼筆記本", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "指揮官A戰術耳機",
                "materials": [
                    {"name": "手寫板", "quantity": 2},
                    {"name": "電容", "quantity": 2}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "輕便型戰術垂直前握把",
                "materials": [
                    {"name": "洗髮精", "quantity": 3},
                    {"name": "氣密性檢測儀", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "AR輕巧戰術前握把",
                "materials": [
                    {"name": "DVD", "quantity": 1},
                    {"name": "保險絲", "quantity": 3}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "P90標準消音器",
                "materials": [
                    {"name": "刷卡器", "quantity": 2},
                    {"name": "鏡頭", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "7.62x51輕量化消音器",
                "materials": [
                    {"name": "機械硬碟", "quantity": 1},
                    {"name": "萬能表", "quantity": 2}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "多功能雷射瞄準器",
                "materials": [
                    {"name": "軍用水壺", "quantity": 2},
                    {"name": "鏡頭", "quantity": 3}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "綠色雷射瞄準器",
                "materials": [
                    {"name": "管鉗", "quantity": 3},
                    {"name": "萬能表", "quantity": 4}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "工業AR後握把",
                "materials": [
                    {"name": "隨身碟", "quantity": 1},
                    {"name": "洗髮精", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "FAL特種槍托",
                "materials": [
                    {"name": "BP機", "quantity": 2},
                    {"name": "損毀的手機", "quantity": 2}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "FAL斜三角槍托",
                "materials": [
                    {"name": "電源線", "quantity": 2},
                    {"name": "晶片處理器", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "FAL帶孔前護木",
                "materials": [
                    {"name": "煤油燈", "quantity": 1},
                    {"name": "紫外線燈", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "P90專用護木",
                "materials": [
                    {"name": "醫用剪刀", "quantity": 4},
                    {"name": "氣體分析儀", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "FAL鏡架機匣蓋",
                "materials": [
                    {"name": "咖啡罐", "quantity": 2},
                    {"name": "搪瓷杯", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
            {
                "name": "M110加長槍管",
                "materials": [
                    {"name": "節能登", "quantity": 2},
                    {"name": "藥物霧化吸入器", "quantity": 1}
                ],
                "description": "兌換人:蘭德爾.費舍"
            },
        ];

        // 搜尋物品
        function searchItem() {
            const searchInput = document.getElementById('searchInput').value.toLowerCase();
            const resultDiv = document.getElementById('searchResult');
            resultDiv.innerHTML = ''; // 清空結果

            if (searchInput.length < 2) {
                resultDiv.innerHTML = "請輸入至少兩個字進行搜尋。";
                return;
            }

            const results = items.filter(item => item.name.toLowerCase().includes(searchInput));

            if (results.length > 0) {
                results.forEach(item => {
                    const itemDiv = document.createElement('div');
                    itemDiv.classList.add('item');
                    let materials = item.materials.map(material => `${material.quantity} x ${material.name}`).join('<br>');
                    itemDiv.innerHTML = `<strong>${item.name}</strong><br>合成材料：<br>${materials}<div class="description">${item.description}</div>`;
                    resultDiv.appendChild(itemDiv);
                });
            } else {
                resultDiv.innerHTML = "沒有找到符合的物品。";
            }
        }

        // 反向搜尋：根據材料搜尋物品
        function searchByMaterials() {
            const materialInput = document.getElementById('materialInput').value.toLowerCase();
            const resultDiv = document.getElementById('materialResult');
            resultDiv.innerHTML = ''; // 清空結果

            if (materialInput.length < 2) {
                resultDiv.innerHTML = "請輸入至少兩個字進行搜尋。";
                return;
            }

            const results = items.filter(item => {
                return item.materials.some(material => material.name.toLowerCase().includes(materialInput));
            });

            if (results.length > 0) {
                results.forEach(item => {
                    const itemDiv = document.createElement('div');
                    itemDiv.classList.add('item');
                    let materials = item.materials.map(material => `${material.quantity} x ${material.name}`).join('<br>');
                    itemDiv.innerHTML = `<strong>${item.name}</strong><br>合成材料：<br>${materials}<div class="description">${item.description}</div>`;
                    resultDiv.appendChild(itemDiv);
                });
            } else {
                resultDiv.innerHTML = "沒有找到符合的材料。";
            }
        }
    </script>

</body>
</html>
