# 結局場景佈置與「完美重置」的重新開始機制

當玩家成功揭開《Winter House》命案的真相後，我們需要給他們一個漂亮的收尾。在這一章中，我們將建立遊戲的最後一站：**End_Scene (結局場景)**，並實作「重新開始」的循環機制。

---

## 🖼️ 第一步：佈置 End_Scene 的沉浸式畫面

結局場景的佈置邏輯與 Start_Scene 非常相似，我們同樣依賴 Canvas 與 2D UI 來呈現。

1. 在 `Project` 視窗的 `_Scenes` 資料夾中，建立並打開名為 **`End_Scene`** 的新場景。
2. 建立 `Canvas`，並**務必記得**將 `Canvas Scaler` 改為 `Scale With Screen Size` (1920x1080)，避免玩家在結局時看到跑版的畫面（詳見第二章）。
3. 加入背景圖片（Image），你可以使用一張 AI 生成的「大雪紛飛的木屋窗外」或是「破曉時分的別墅」作為破關背景。
4. 加入 `Text (TMP)`，輸入結局的描述文字，例如：*「你發現了杯底的粉末。原來這場讀書會，從一開始就是為他設下的死亡陷阱...」*。記得套用我們在第一章做好的「動態中文字體 (Dynamic)」。

---

## 🔄 第二步：建立「重新開始」按鈕與腳本

讓玩家破關後只能強制關閉遊戲是非常破壞體驗的。我們需要一個按鈕，讓他們一鍵回到起點。

1. 在 Canvas 下建立一個 `Button - TextMeshPro`，將文字改為 **「重新開始」** 或 **「再次調查」**。
2. 在 `Scripts` 資料夾中，新建一個名為 `SceneController`（或直接寫在該場景的 GameManager 裡）的 C# 腳本。

打開腳本，寫入以下這段超級簡潔的換場程式碼：

```csharp
using UnityEngine;
using UnityEngine.SceneManagement; // ⚠️ 必須引入這個才能切換場景！

public class SceneController : MonoBehaviour
{
    // 這個方法要設定為 public，按鈕才能呼叫到它
    public void RestartGame()
    {
        // 讀取名為 Start_Scene 的場景
        SceneManager.LoadScene("Start_Scene");
    }
}
