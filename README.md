# SunShaft 光线

基于 URP ScriptableRendererFeature 的太阳光轴（体积光）后处理效果示例工程。

效果文档参考：[SunShaft 光线](https://github.com/xieliujian/com.spacetime.effect/blob/main/readme/SunShaft/SunShaft.md)

---

## 快速开始

### 第一步：添加 Renderer Feature

在 URP Renderer Asset 中添加 `SunShaftsFeatureV2`：

```
Universal Renderer Data > Add Renderer Feature > SunShaftsFeatureV2
```

### 第二步：添加 Volume 组件

在场景中创建 Volume（Global 或 Local），添加 `Post-processing > SunShafts` Override，将 `On` 开启即可启用效果。

### 第三步：配置参数

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `On` | false | 启用/禁用效果 |
| `Force On` | false | 强制开启，跳过角度可见性裁剪 |
| `Intensity` | 1.5 | 光柱强度（0–5） |
| `Use Sun Light Color` | true | 使用主方向光颜色作为光柱颜色 |
| `Shafts Color` | black | 自定义光柱颜色（HDR） |
| `Use Sun Position` | false | 手动指定太阳位置 |
| `Sun Position` | Vector3.zero | 自定义太阳世界坐标 |
| `Sun Threshold Sky` | 0.75 | 天空采样范围阈值（0–1） |
| `Depth Downscale Pow2` | 3 | 降采样级别（0–4） |
| `Blur Radius` | 1.2 | 径向模糊半径 |
| `Blur Steps Count` | 2 | 径向模糊迭代次数（1–4） |
| `Use Stencil Mask Tex` | false | 启用模板遮罩纹理 |
| `Use Render Pass Event` | false | 自定义 RenderPass 插入时机 |

---

## 已知问题

### WebGL 打包后效果不显示

**原因**：Unity WebGL 打包时会裁剪未被任何序列化资源引用的 Shader。SunShaft 的三个 Shader 通过 `Shader.Find()` 在运行时查找，不被任何 `.mat` 引用，打包后被剔除，导致材质创建失败、效果不渲染。

**修复**：在 `ProjectSettings/GraphicsSettings.asset` 的 `m_AlwaysIncludedShaders` 中加入三个 Shader（已修复）：

| Shader | 路径 |
|--------|------|
| BuildSkyForBlurShader | `SpaceTime/PostProcess/SunShaft/BuildSkyForBlurShader` |
| DirectionalBlurShader | `SpaceTime/PostProcess/SunShaft/DirectionalBlurShader` |
| FinalBlendShader | `SpaceTime/PostProcess/SunShaft/FinalBlendShader` |

等效操作：`Edit → Project Settings → Graphics → Always Included Shaders` 手动添加上述三个 Shader。
