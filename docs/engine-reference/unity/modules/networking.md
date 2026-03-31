# Unity 2023.2 — Networking 模块参考

**最后验证:** 2026-03-31

---

## 概述

Unity 2023.2 网络选项：
- **Netcode for GameObjects**（推荐）：Unity 官方多人框架
- **Mirror**: 社区驱动（UNet 继承者）
- **Photon**: 第三方服务（PUN2）

**UNet (遗留)**: 已废弃，不要使用。

---

## Netcode for GameObjects

### 安装

1. `Window > Package Manager`
2. 搜索 "Netcode for GameObjects"
3. 安装 `com.unity.netcode.gameobjects`

---

## 基础设置

### NetworkManager

```csharp
using Unity.Netcode;

public class CustomNetworkManager : MonoBehaviour {
    void Start() {
        NetworkManager.Singleton.StartHost(); // 服务器 + 客户端
        // 或
        NetworkManager.Singleton.StartServer(); // 专用服务器
        // 或
        NetworkManager.Singleton.StartClient(); // 仅客户端
    }
}
```

---

## NetworkObject（网络化 GameObject）

### 标记 GameObject 为网络化

1. 添加 `NetworkObject` 组件到 GameObject
2. 必须在预制体根部（不是嵌套的）
3. 在 `NetworkManager > NetworkPrefabs List` 注册预制体

### 生成网络对象

```csharp
using Unity.Netcode;

public class GameManager : NetworkBehaviour {
    public GameObject playerPrefab;

    [ServerRpc(RequireOwnership = false)]
    public void SpawnPlayerServerRpc(ulong clientId) {
        GameObject player = Instantiate(playerPrefab);
        player.GetComponent<NetworkObject>().SpawnAsPlayerObject(clientId);
    }
}
```

---

## NetworkBehaviour（网络化脚本）

### NetworkBehaviour 基类

```csharp
using Unity.Netcode;

public class Player : NetworkBehaviour {
    public override void OnNetworkSpawn() {
        if (IsOwner) {
            GetComponent<Camera>().enabled = true;
        }
    }

    void Update() {
        if (!IsOwner) return;
        if (Input.GetKey(KeyCode.W)) {
            MoveServerRpc(Vector3.forward);
        }
    }

    [ServerRpc]
    void MoveServerRpc(Vector3 direction) {
        transform.position += direction * Time.deltaTime;
    }
}
```

---

## Network Variables（同步状态）

### NetworkVariable<T>

```csharp
using Unity.Netcode;

public class Player : NetworkBehaviour {
    private NetworkVariable<int> health = new NetworkVariable<int>(100);

    public override void OnNetworkSpawn() {
        health.OnValueChanged += OnHealthChanged;
    }

    void OnHealthChanged(int oldValue, int newValue) {
        Debug.Log($"生命值变化: {oldValue} -> {newValue}");
    }

    [ServerRpc]
    public void TakeDamageServerRpc(int damage) {
        health.Value -= damage;
    }
}
```

---

## RPCs（远程过程调用）

### ServerRpc（客户端 → 服务器）

```csharp
[ServerRpc]
void FireWeaponServerRpc() {
    Debug.Log("服务器: 武器开火");
}

// 从客户端调用:
if (IsOwner && Input.GetKeyDown(KeyCode.Space)) {
    FireWeaponServerRpc();
}
```

### ClientRpc（服务器 → 所有客户端）

```csharp
[ClientRpc]
void PlayExplosionClientRpc(Vector3 position) {
    Instantiate(explosionPrefab, position, Quaternion.identity);
}

[ServerRpc]
void ExplodeServerRpc(Vector3 position) {
    DealDamageToNearbyPlayers(position);
    PlayExplosionClientRpc(position);
}
```

---

## 网络所有权

### 检查所有权

```csharp
if (IsOwner) { }      // 此客户端拥有此 NetworkObject
if (IsServer) { }     // 运行在服务器上
if (IsClient) { }     // 运行在客户端上
if (IsLocalPlayer) { } // 这是本地玩家对象
```

---

## 调试

### Network Profiler

- `Window > Analysis > Network Profiler` — 监控带宽、RPC 调用、变量更新

### Network Simulator（测试延迟/丢包）

- `NetworkManager > Network Simulator` — 添加人工延迟和丢包进行测试

---

## 参考来源

- https://docs-multiplayer.unity3d.com/netcode/current/about/
