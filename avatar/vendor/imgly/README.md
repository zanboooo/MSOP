# 镜像：@imgly/background-removal-data 1.7.0

本目录是 [@imgly/background-removal](https://github.com/imgly/background-removal-js) 模型数据包的
**未修改镜像**，由 `AI CEO/tools/mirror_imgly.js` 生成，每个分块按上游清单的 SHA-256 逐个校验。

## 为什么镜像

上游 `staticimgly.com` 对模型文件返回 `cache-control: max-age=14400` 且**不带 ETag**。
没有 ETag 就无法协商缓存 —— 4 小时一过浏览器只能整包重下（默认 fp16 模型 84MB + wasm 11MB）。
GitHub Pages 带 ETag 与 Last-Modified，过期后走 304 空响应，**首次之后不再重复下载**。

## 镜像了什么

| 资源 | 大小 | 说明 |
|---|---|---|
| `models/isnet_quint8` | 42.3 MB | 备选，`?m=small` 切换 |
| `models/isnet_fp16` | 84.1 MB | **本页默认**（同图实测背景残留 0.00%，quint8 为 7.05%）|
| `onnxruntime-web/ort-wasm-simd-threaded.wasm` | 11.3 MB | CPU 推理 |
| `onnxruntime-web/ort-wasm-simd-threaded.mjs` | 21 KB | |

未镜像：`isnet`（167MB，收益不抵体积）、`*.jsep.*`（WebGPU 专用，本页 `device` 用默认 `cpu`，不会请求）。

分块以内容哈希为文件名、每块 4MB，由 `resources.json` 索引。`resources.json` 已裁剪成只含以上四项。

## 许可

模型与运行时依 **AGPL-3.0** 授权，版权归 IMG.LY GmbH。本目录为原样再分发，未作修改。
许可全文见 https://github.com/imgly/background-removal-js/blob/main/LICENSE
调用它的页面源码在本仓库 `avatar/index.html`，公开可取（AGPL §13）。
