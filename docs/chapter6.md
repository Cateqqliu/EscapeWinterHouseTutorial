# 第六章：喚醒聽覺！背景音樂與互動音效實裝

在《Winter House》中，呼嘯的寒風與道具碰撞的清脆聲響，是建立「密室逃脫沉浸感」的兩大支柱。我們在第零章已經準備好了音檔，現在要把這些 `.mp3` 和 `.wav` 真正掛載到 Unity 的場景中。

這一章，我們將處理持續播放的「背景音樂 (BGM)」，以及點擊道具時觸發的「互動音效 (SFX)」。

---

## 🎵 第一步：設置環境背景音樂 (BGM)

背景音樂最簡單，它只需要一個喇叭，並且在遊戲開始時自動、無縫地重複播放即可。

1. 在 `Play_Scene` 場景左側的 `Hierarchy` 中，點選你的 `--- Managers ---` 空物件。
2. 在它底下按右鍵 ➔ `Create Empty`，建立一個子物件並命名為 **`BGM_Manager`**。
3. 點選 `BGM_Manager`，在右側 `Inspector` 點擊 `Add Component`，加入 **`Audio Source`**（這是 Unity 的虛擬喇叭）。
4. **關鍵設定：**
   * **`AudioClip`**：把你準備好的背景音樂檔（例如 `Winter_BGM.mp3`）拖曳到這個欄位裡。
   * **`Play On Awake`**：確認有**勾選**（遊戲一執行就會自動播放）。
   * **`Loop`**：務必**勾選**！（這樣音樂播完才會自動從頭開始，達成無縫循環）。
   * **`Volume` (音量)**：AI 生成的音樂通常很大聲，建議先調小到 `0.3` 左右，才不會蓋過遊戲音效。

---

## 🔊 第二步：利用 GameManager 打造「音效播放中心」

對於短暫的音效（例如點擊馬克杯的「喀啦」聲），如果我們在每個道具上都裝一個喇叭，會非常浪費效能，而且很難統一管理音量。

最聰明的做法，是讓大總管 `GameManager` 兼任「音效播放中心」。只要道具被點擊，就把音效檔傳給大總管，請它代為播放！

請打開你的 **`GameManager`** 腳本，加入兩行關於音效的新程式碼（標示為 `// 新增` 的部分）：

```csharp
using UnityEngine;
using TMPro; 

public class GameManager : MonoBehaviour
{
    public static GameManager Instance;

    [Header("UI 綁定")]
    public GameObject dialoguePanel; 
    public TMP_Text dialogueText;    

    [Header("音效綁定")] // 新增：用來分類介面
    public AudioSource sfxPlayer;    // 新增：大總管專屬的音效喇叭

    private void Awake()
    {
        Instance = this; 
    }

    private void Start()
    {
        dialoguePanel.SetActive(false);
    }

    public void ShowDialogue(string content)
    {
        dialogueText.text = content; 
        dialoguePanel.SetActive(true); 
    }

    public void CloseDialogue()
    {
        dialoguePanel.SetActive(false);
    }

    // 新增：開放給所有道具呼叫的「播放音效」功能
    public void PlaySFX(AudioClip clip)
    {
        if (clip != null)
        {
            sfxPlayer.PlayOneShot(clip); // PlayOneShot 可以讓音效重疊播放，不會互相切斷！
        }
    }
}
