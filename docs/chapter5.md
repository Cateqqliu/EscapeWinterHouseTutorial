# 第五章：實體道具互動！Box Collider 與對話框的完美串接

在第二章我們把 Play 場景的對話框與 UI 排版固定好了；現在，我們要讓場景中的實體道具（例如那把刀子、馬克杯）擁有被點擊的能力，並且能在點擊後，呼叫大總管 `GameManager` 彈出對話框。

這是一個經典的「點擊解謎 (Point-and-Click)」互動迴圈！

---

## 🛡️ 第一步：確保道具有「物理防護罩」

在 Unity 中，滑鼠是點不到一張「純圖片」的，它只能點擊到「碰撞體 (Collider)」。

1. 點選 `--- Interactables ---` 資料夾底下的道具（例如 `Mug` 馬克杯）。
2. 在右側 `Inspector` 點擊 `Add Component`，加入 **`Box Collider 2D`**（如果是 3D 專案請選 `Box Collider`）。
3. 點擊 `Edit Collider` 的小圖示，調整綠色框框，讓它完美包覆住馬克杯。
   *(這塊綠色區域，就是未來玩家滑鼠可以點擊的感應區！)*

---

## 📜 第二步：撰寫道具專屬腳本 (InteractableItem)

接下來，我們要給這個馬克杯大腦，讓它知道「被點擊時要說什麼話」。

1. 在 `Scripts` 資料夾建立新 C# 腳本，命名為 **`InteractableItem`**。
2. 將腳本拖曳掛載到馬克杯上。
3. 雙擊打開腳本，寫入以下程式碼：

```csharp
using UnityEngine;

public class InteractableItem : MonoBehaviour
{
    [Header("道具設定")]
    public string itemName; // 道具名稱
    [TextArea] // 讓文字輸入框變大，方便打很多字
    public string description; // 點擊後要顯示的對話內容

    // 這是 Unity 內建的神奇方法！只要物件有 Collider 且被滑鼠點擊，就會自動執行
    private void OnMouseDown()
    {
        Debug.Log("玩家點擊了：" + itemName);
        
        // 呼叫 GameManager，把對話內容傳過去請它顯示
        GameManager.Instance.ShowDialogue(description);
    }
}
