# 第四章：第一幕的雙雄連線！GameManager 與 SceneLoader

在《Winter House》的開始畫面（第一幕）中，舞台上雖然還沒有複雜的解謎機關，但底層已經有兩位最核心的「幕後工作人員」悄悄進駐了。它們分別是掌管全域狀態的 **`GameManager`**，以及負責開啟傳送門的 **`SceneLoader`**。

這一章我們將拆解這兩個腳本在第一幕的分工，並正式讓開始按鈕活過來！

---

## 🏗️ 1. 第一幕的幕後團隊配置

在左側的 `Hierarchy` 視窗中，我們通常會在 **`--- Managers ---`** 這個空物件底下，掛載這兩個最重要的 C# 腳本：

* **`GameManager.cs`**：遊戲的大總管。雖然第一幕還不需要數馬克杯或開對話框，但大總管會在這裡率先誕生，初始化遊戲的基本設定（例如確保一開始的背包是空的），靜靜等待玩家按下開始。
* **`SceneLoader.cs`**：專職的接線生。它的職責非常單純且專一——只要有人呼叫它，它就會立刻把玩家載入到指定的場景。

---

## 📜 2. 撰寫 SceneLoader 換場腳本

請在 `Scripts` 資料夾中建立名為 **`SceneLoader`** 的 C# 腳本，雙擊打開並寫入以下代碼：

```csharp
using UnityEngine;
// ⚠️ 這是控制場景切換必須引入的精靈！
using UnityEngine.SceneManagement;

public class SceneLoader : MonoBehaviour
{
    // 必須設定為 public，外面的 UI 按鈕才能點擊到它
    public void StartGame()
    {
        Debug.Log("Winter House 門戶已開，準備進入 Play 場景...");
        
        // 載入名為 Play_Scene 的遊玩場景
        SceneManager.LoadScene("Play_Scene"); 
    }
}
