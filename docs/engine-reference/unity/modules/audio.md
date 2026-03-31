# Unity 2023.2 — 音频模块参考

**最后验证:** 2026-03-31

---

## 概述

Unity 2023.2 音频系统：
- **AudioSource**: 在 GameObject 上播放声音
- **Audio Mixer**: 混音、效果处理、动态混音
- **Spatial Audio**: 3D 定位声音

---

## 基础音频播放

### AudioSource 组件

```csharp
AudioSource audioSource = GetComponent<AudioSource>();

// ✅ 播放
audioSource.Play();

// ✅ 延迟播放
audioSource.PlayDelayed(0.5f);

// ✅ 单次播放（不中断当前声音）
audioSource.PlayOneShot(clip);

// ✅ 停止
audioSource.Stop();

// ✅ 暂停/继续
audioSource.Pause();
audioSource.UnPause();
```

### 在位置播放声音（静态方法）

```csharp
// ✅ 快速 3D 声音播放（完成后自动销毁）
AudioSource.PlayClipAtPoint(clip, transform.position);

// ✅ 带音量
AudioSource.PlayClipAtPoint(clip, transform.position, 0.7f);
```

---

## 3D 空间音频

### AudioSource 3D 设置

```csharp
AudioSource source = GetComponent<AudioSource>();

// Spatial Blend: 0 = 2D, 1 = 3D
source.spatialBlend = 1.0f; // 完全 3D

// 多普勒效应（基于速度的音调偏移）
source.dopplerLevel = 1.0f;

// 距离衰减
source.minDistance = 1f;   // 此距离内完整音量
source.maxDistance = 50f; // 超出此距离无声
source.rolloffMode = AudioRolloffMode.Logarithmic; // 自然衰减
```

---

## Audio Mixer（高级混音）

### 设置 Audio Mixer

1. `Assets > Create > Audio Mixer`
2. 打开混音器: `Window > Audio > Audio Mixer`
3. 创建组: Master > SFX, Music, Dialogue

### 分配 AudioSource 到 Mixer 组

```csharp
using UnityEngine.Audio;

public AudioMixerGroup sfxGroup;

void Start() {
    AudioSource source = GetComponent<AudioSource>();
    source.outputAudioMixerGroup = sfxGroup;
}
```

### 从代码控制 Mixer

```csharp
using UnityEngine.Audio;

public AudioMixer audioMixer;

// ✅ 设置音量（暴露参数）
audioMixer.SetFloat("MusicVolume", -10f); // dB (-80 到 0)

// ✅ 获取音量
audioMixer.GetFloat("MusicVolume", out float volume);
```

---

## 音频效果

### 添加效果到 Mixer 组

在 Audio Mixer 中：
- 点击组（例如 SFX）
- 点击 "Add Effect"
- 选择: Reverb, Echo, Low Pass, High Pass, Distortion 等

---

## 音频性能

### 优化音频加载

```csharp
// 音频导入设置 (Inspector):
// - Load Type:
//   - Decompress On Load: 小型音频（SFX），完全加载到内存
//   - Compressed In Memory: 中型音频，运行时解压（推荐）
//   - Streaming: 大型音频（音乐），从磁盘流式传输

// 压缩格式:
// - PCM: 未压缩，最高音质，最大体积
// - ADPCM: 3.5x 压缩，适合 SFX
// - Vorbis/MP3: 高压缩，适合音乐
```

---

## 常见模式

### 随机音高变化（避免重复）

```csharp
void PlaySoundWithVariation(AudioClip clip) {
    AudioSource source = GetComponent<AudioSource>();
    source.pitch = Random.Range(0.9f, 1.1f);
    source.PlayOneShot(clip);
}
```

### 脚步声（从数组随机）

```csharp
public AudioClip[] footstepClips;

void PlayFootstep() {
    AudioClip clip = footstepClips[Random.Range(0, footstepClips.Length)];
    AudioSource.PlayClipAtPoint(clip, transform.position, 0.5f);
}
```

---

## Audio Listener

### 单一 Listener 规则

- 一次只能有一个 `AudioListener` 处于活动状态
- 通常附加到主相机

---

## 调试

- `Window > Audio > Audio Mixer` — 可视化电平，测试快照
- `Edit > Project Settings > Audio` — 全局音量、DSP 缓冲区大小、扬声器模式

---

## 参考来源

- https://docs.unity3d.com/2023.2/Documentation/Manual/Audio.html
