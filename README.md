# GMK KB01 Sound Drivers / GMK KB01 音效驅動程式

Personal archive of the factory sound drivers for the **GMK KB01** mini laptop (also referred to as **GB01** in the factory recovery image) on **Windows 11**.

本專案為 **GMK KB01** 迷你筆記型電腦（原廠映像亦稱為 **GB01**）於 **Windows 11** 使用的原廠音效驅動程式個人存檔。

---

## English

### What this is

These are the OEM sound drivers for the GMK KB01, extracted from the factory recovery image:

- Package: `GB01_En-13Langs_Home_v1-1_22000-318.zip`
- Image: Windows 11 Home, build **22000.318** (captured 2022-08-30)
- Source: INF-style packages from the factory image DriverStore (there is no `setup.exe`)

The analog codec is an **Everest Semiconductor ES8326 / ES8336** (ACPI IDs `ESSX8326` / `ESSX8336`). It sits behind **Intel Smart Sound Technology (SST)** on a Gemini Lake SST controller (**PCI `VEN_8086` `DEV_3198`**). Both the Everest codec package and the Intel SST stack are required for speakers, headphones, and related audio devices.

These files are vendor / OEM drivers. This repository is a **personal archive** for installing them on a GMK KB01. It is not an official Intel, Everest Semiconductor, or GMK release, and it does not grant an open-source licence. See [NOTICE](NOTICE).

Windows 11 may warn that the drivers are older or were not written specifically for a later Windows 11 build. That is expected: they came from the factory 22000.318 image for this model.

### What is included

**Everest Semiconductor codec**

| Folder | INF | Version | Role |
| --- | --- | --- | --- |
| `01-Everest-ESAuDriver` | `ESAuDriver.inf` | 1.1.0.1 (2021-09-24) | Codec function driver for ES8326 / ES8336 |
| `02-Everest-APO` | `EverestApo.inf` | 17.36.19.705 (2021-09-13) | Audio Processing Object (effects) |
| `03-Everest-AudioService` | `ESAudioServiceInstall.inf`, `ESAudioServiceExtension.inf` | 2.0.1.2 (2021-11-18) | Everest audio service and extension |
| `09-Everest-ISST-APO-Extension` | `ISSTApoExt.inf` (two versions) | 17.36.19.752 (2021-09-13) and 19.13.58.464 (2020-12-23) | Intel SST APO extensions from Everest |

**Intel Smart Sound Technology stack**

| Folder | INF | Version | Role |
| --- | --- | --- | --- |
| `04-Intel-SST-AudioBus` | `IntcAudioBus.inf` | 09.22.00.4078 (2019-01-23) | Intel SST audio bus / controller (PCI, including `VEN_8086&DEV_3198`) |
| `05-Intel-SST` | `IntcSST.inf` | 09.22.00.4078 (2019-01-23) | Intel SST audio DSP function driver |
| `06-Intel-SST-DisplayAudio` | `intcdaud.inf` | 10.26.0.6 (2019-01-08) | Intel Display Audio (HDMI / DisplayPort) |
| `07-Intel-SST-DigitalMic` | `IntcDMic.inf` | 09.22.00.4078 (2019-01-23) | Intel SST digital microphone |
| `08-Intel-SST-OED` | `IntcOED.inf` | 09.22.00.4078 (2019-01-23) | Intel SST Offload Engine Driver (Intel High Definition DSP), including firmware and wake-on-voice (WoV) artifact files |

The OED folder is the largest package. The WoV models (for example Echo / Alexa and Cortana-related assets) are included here because the Intel High Definition DSP package can fail to install if those artifact files are missing. They are not required just to play sound through speakers or headphones.

### How to install

Do this as an administrator. Restart after a successful install.

#### Method A — Device Manager (recommended)

1. Right-click Start → **Device Manager**.
2. Look under **Other devices**, **Sound, video and game controllers**, or **System devices** for entries such as:
   - Multimedia Audio Controller
   - High Definition Audio Controller
   - Intel Smart Sound Technology
   - ESSX8326 / ESSX8336 / ESAuDriver Device
3. Right-click the unknown or problematic audio device → **Update driver** → **Browse my computer for drivers**.
4. Browse to this `gmk-kb01-sound-driver` folder.
5. Tick **Include subfolders** and click Next.
6. Repeat for any remaining audio-related unknown devices.
7. Restart the PC.

#### Method B — pnputil (install all INFs at once)

1. Open Command Prompt or PowerShell **as Administrator**.
2. Run (replace `<path>` with the folder that contains these packages):

```bat
pnputil /add-driver "<path>\*.inf" /subdirs /install
```

Example, if the folder is on the Desktop:

```bat
pnputil /add-driver "C:\Users\<you>\Desktop\gmk-kb01-sound-driver\*.inf" /subdirs /install
```

3. Restart the PC.

#### Suggested order if installing one-by-one

If a bulk install fails, or you prefer to install packages individually (right-click an INF and choose Install, or run `pnputil` on that folder only), use this order:

1. `04-Intel-SST-AudioBus` (`IntcAudioBus.inf`) — install this first
2. `08-Intel-SST-OED` (`IntcOED.inf`)
3. `05-Intel-SST` (`IntcSST.inf`)
4. `07-Intel-SST-DigitalMic` (`IntcDMic.inf`)
5. `06-Intel-SST-DisplayAudio` (`intcdaud.inf`)
6. `01-Everest-ESAuDriver` (`ESAuDriver.inf`)
7. `02-Everest-APO` (`EverestApo.inf`)
8. `03-Everest-AudioService` (both INFs)
9. `09-Everest-ISST-APO-Extension` (both version folders)

### Notes from an install on a GMK KB01 (Windows 11)

- The Everest **ES8336** codec and the **Intel SST Audio Controller** installed successfully.
- **IntcOED** / **Intel High Definition DSP** can fail if the WoV artifact files are missing. This archive includes those files under `08-Intel-SST-OED\WoVartifacts`.
- After the core packages are in place, **HDMI display audio** may appear as a playback device before the analog speakers or headphones show up.
- A **restart** is recommended before judging whether speakers and headphones work.
- Windows 11 may warn that these drivers are older. If Windows Update later offers a newer Intel SST or Everest package, you can try that after this install.

### Disclaimer

These binaries remain copyright Intel, Everest Semiconductor, and the original OEM. This archive is for personal use with the GMK KB01 only. No warranty is provided. Use at your own risk. See [NOTICE](NOTICE).

---

## 繁體中文（香港）

### 這是甚麼

這些是 GMK KB01 的原廠（OEM）音效驅動程式，由原廠還原映像抽出：

- 套件：`GB01_En-13Langs_Home_v1-1_22000-318.zip`
- 映像：Windows 11 Home，版本 **22000.318**（擷取日期 2022-08-30）
- 來源：原廠映像 DriverStore 內的 INF 驅動套件（沒有 `setup.exe`）

類比音效編碼器為 **Everest Semiconductor ES8326 / ES8336**（ACPI 識別碼 `ESSX8326` / `ESSX8336`）。它位於 Gemini Lake 的 **Intel Smart Sound Technology (SST)** 控制器之後（**PCI `VEN_8086` `DEV_3198`**）。揚聲器、耳機及相關音效裝置須同時安裝 Everest 編碼器及 Intel SST 套件。

這些檔案屬於供應商／原廠驅動程式。本專案只是供 GMK KB01 安裝之用的**個人存檔**，並非 Intel、Everest Semiconductor 或 GMK 的官方發佈，亦**不授予**開放原始碼授權。詳情見 [NOTICE](NOTICE)。

Windows 11 可能會提示這些驅動程式較舊，或並非為較新的 Windows 11 版本編寫。此屬預期情況：檔案來自此型號的原廠 22000.318 映像。

### 包含內容

**Everest Semiconductor 編碼器**

| 資料夾 | INF | 版本 | 用途 |
| --- | --- | --- | --- |
| `01-Everest-ESAuDriver` | `ESAuDriver.inf` | 1.1.0.1（2021-09-24） | ES8326 / ES8336 編碼器功能驅動程式 |
| `02-Everest-APO` | `EverestApo.inf` | 17.36.19.705（2021-09-13） | 音效處理物件（效果） |
| `03-Everest-AudioService` | `ESAudioServiceInstall.inf`、`ESAudioServiceExtension.inf` | 2.0.1.2（2021-11-18） | Everest 音效服務及延伸套件 |
| `09-Everest-ISST-APO-Extension` | `ISSTApoExt.inf`（兩個版本） | 17.36.19.752（2021-09-13）及 19.13.58.464（2020-12-23） | Everest 提供的 Intel SST APO 延伸 |

**Intel Smart Sound Technology 套件**

| 資料夾 | INF | 版本 | 用途 |
| --- | --- | --- | --- |
| `04-Intel-SST-AudioBus` | `IntcAudioBus.inf` | 09.22.00.4078（2019-01-23） | Intel SST 音效匯流排／控制器（PCI，包括 `VEN_8086&DEV_3198`） |
| `05-Intel-SST` | `IntcSST.inf` | 09.22.00.4078（2019-01-23） | Intel SST 音效 DSP 功能驅動程式 |
| `06-Intel-SST-DisplayAudio` | `intcdaud.inf` | 10.26.0.6（2019-01-08） | Intel 顯示音效（HDMI / DisplayPort） |
| `07-Intel-SST-DigitalMic` | `IntcDMic.inf` | 09.22.00.4078（2019-01-23） | Intel SST 數碼咪高峰 |
| `08-Intel-SST-OED` | `IntcOED.inf` | 09.22.00.4078（2019-01-23） | Intel SST Offload Engine Driver（Intel High Definition DSP），包括韌體及語音喚醒（WoV）檔案 |

OED 資料夾是最大的套件。語音喚醒模型（例如 Echo／Alexa 及 Cortana 相關檔案）已包含在內，因為若欠缺這些檔案，Intel High Definition DSP 套件可能會安裝失敗。僅使用揚聲器或耳機播放時，並不一定需要用到這些語音喚醒功能。

### 安裝方法

請以系統管理員身分安裝，成功後重新開機。

#### 方法 A — 裝置管理員（建議）

1. 在「開始」按鈕按滑鼠右鍵，開啟**裝置管理員**。
2. 在「**其他裝置**」、「**音效、視訊及遊戲控制器**」或「**系統裝置**」尋找下列項目：
   - Multimedia Audio Controller（多媒體音效控制器）
   - High Definition Audio Controller
   - Intel Smart Sound Technology
   - ESSX8326 / ESSX8336 / ESAuDriver Device
3. 在尚未識別或有問題的音效裝置按右鍵 → **更新驅動程式** → **瀏覽我的電腦以尋找驅動程式**。
4. 指向這個 `gmk-kb01-sound-driver` 資料夾。
5. 勾選「**包含子資料夾**」，然後按「下一步」。
6. 如仍有其他未安裝的音效裝置，請重複以上步驟。
7. 重新開機。

#### 方法 B — pnputil（一次過安裝所有 INF）

1. 以**系統管理員**身分開啟命令提示字元或 PowerShell。
2. 執行以下指令（將 `<path>` 改為這個資料夾的實際路徑）：

```bat
pnputil /add-driver "<path>\*.inf" /subdirs /install
```

如資料夾位於桌面，可參考以下例子：

```bat
pnputil /add-driver "C:\Users\<你的使用者名稱>\Desktop\gmk-kb01-sound-driver\*.inf" /subdirs /install
```

3. 重新開機。

#### 如需逐個安裝，建議次序

如一次過安裝失敗，或希望逐個套件安裝（在 INF 檔按右鍵選「安裝」，或只對該資料夾執行 `pnputil`），請按下列次序：

1. `04-Intel-SST-AudioBus`（`IntcAudioBus.inf`）——請先安裝此套件
2. `08-Intel-SST-OED`（`IntcOED.inf`）
3. `05-Intel-SST`（`IntcSST.inf`）
4. `07-Intel-SST-DigitalMic`（`IntcDMic.inf`）
5. `06-Intel-SST-DisplayAudio`（`intcdaud.inf`）
6. `01-Everest-ESAuDriver`（`ESAuDriver.inf`）
7. `02-Everest-APO`（`EverestApo.inf`）
8. `03-Everest-AudioService`（兩個 INF）
9. `09-Everest-ISST-APO-Extension`（兩個版本資料夾）

### 在一部 GMK KB01（Windows 11）實際安裝時的情況

- Everest **ES8336** 編碼器及 **Intel SST Audio Controller** 已成功安裝。
- 如欠缺語音喚醒（WoV）檔案，**IntcOED**／**Intel High Definition DSP** 可能會安裝失敗。本存檔已將這些檔案放在 `08-Intel-SST-OED\WoVartifacts`。
- 核心套件安裝完成後，**HDMI 顯示音效**可能會先出現，類比揚聲器或耳機或須稍後（或重新開機後）才會顯示。
- **建議重新開機**，再判斷揚聲器及耳機是否正常。
- Windows 11 可能會提示這些驅動程式較舊。如之後 Windows Update 提供較新的 Intel SST 或 Everest 套件，可在今次安裝之後再試。

### 免責聲明

這些二進位檔案的版權仍屬 Intel、Everest Semiconductor 及原廠所有。本存檔僅供 GMK KB01 個人使用。並不提供任何保證，使用風險自負。詳情見 [NOTICE](NOTICE)。
