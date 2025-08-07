Mushroom picking is a popular activity in Finland. However, for many foreigners (like me), identifying the right mushrooms in the forest can be confusing.

**MushFinder** is an **experimental tool** I built to help users **get a rough suggestion** of what kind of mushroom they're seeing (*not to determine if it's edible*), based on a photo taken with their phone. It's powered by a custom CNN model and focuses specifically on mushrooms commonly found in Finnish forests.

This is my first full machine learning project, and it's currently in the testing phase.

---

> ⚠️ **Important Note**: The identification results are for **reference only**. This tool may **NOT** be accurate enough for food safety decisions.
>
> ☠️☠️☠️ Consuming wild mushrooms based on this tool’s predictions can be **extremely dangerous and potentially fatal**. You are **fully responsible** for any decisions you make.
>
> **NEVER** consume a mushroom unless it has been confirmed as safe by a **qualified expert**, or unless you are **100% certain** of its identity.
>
> 📢 **Disclaimer**: This tool is the result of an experimental machine learning project. The training process may contain **flaws** or **limitations** that affect model performance.
>
> While the model achieved a certain level of validation accuracy under experimental conditions, this figure was obtained on a controlled dataset and **does not reflect real world performance**.
>
> In actual use, factors like lighting conditions, camera angle, image quality, and unfamiliar mushroom species can significantly reduce prediction accuracy. The results should therefore be treated with caution and **must not be relied upon for food safety decisions**.
>
> This software is provided **“as is”, without warranty of any kind**, express or implied. The developer assumes **no responsibility** for any consequences arising from the use of this tool.

---

## ✨ Features

- **Local Focus**
  Unlike Google Lens or other general-purpose tools, MushFinder is trained to recognize just the **20 most common mushroom species** in Finland. This makes it more efficient for the local environment.

- **Online / Offline Versions**
  - **Online version**: Achieves ~85% accuracy on validation data.
    *(Note: this number was obtained on a controlled dataset and does not reflect real world performance.)* Best suited for use with internet access. 
  - **Edge version**: Much smaller, designed to run **entirely offline**, suitable for remote forests where mobile signal is weak.

- **Easy to Use**
  - Works directly in the browser, no installation needed.
  - Optimized for mobile devices with camera support.
  - Runs on all major platforms (Android / iOS / Desktop).

- **Privacy Friendly**
  - No personal data or image content is collected or stored.
  - All uploaded images are **automatically deleted daily** from the server.
  - In the offline version, images are never uploaded, all processing runs locally on the device.

---

## 🧪 Tech Highlights

  - Built with **transfer learning**; tested ~10 CNN architectures and selected **ConvNeXt**.
    - Online version uses **ConvNeXt-Tiny**.
    - Offline version uses **ConvNeXt-Pico**, optimized for edge use.
  - Used **Optuna** for automated hyperparameter tuning.
  - Designed a **custom loss function** to penalize toxic mushroom misclassification more heavily.(However, this approach still **does not guarantee safety or reliability** in real world use.)

---

## 🚧 Status

The project is deploying, more details will be published soon.

Stay tuned!
