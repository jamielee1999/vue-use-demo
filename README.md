# VueUse 功能示範

這是一個使用 VueUse 的示範專案，展示了 VueUse 的一些常用功能。

## 功能展示

### 1. 滑鼠位置追蹤
- 使用 `useMouse` 追蹤滑鼠位置
- 即時顯示滑鼠座標
- 支援相對定位

### 2. 本地存儲
- 使用 `useLocalStorage` 實現數據持久化
- 自動同步到 localStorage
- 支援 TypeScript 類型

### 3. 視窗大小
- 使用 `useWindowSize` 監聽視窗大小變化
- 即時顯示視窗尺寸
- 響應式更新

### 4. 防抖動搜尋
- 使用 `useDebounce` 實現搜尋防抖
- 優化用戶輸入體驗
- 減少不必要的請求

### 5. 異步狀態管理
- 使用 `useAsyncState` 管理異步操作
- 支援載入狀態和錯誤處理
- 提供自動執行和手動觸發選項

## 技術棧

- Vue 3
- TypeScript
- VueUse
- Tailwind CSS

## 開發環境設置

```bash
# 安裝依賴
npm install

# 啟動開發服務器
npm run dev

# 構建生產版本
npm run build
```

## 專案結構

```
src/
  ├── components/
  │   ├── VueUseDemo.vue    # 主要功能示範
  │   └── AsyncStateDemo.vue # 異步狀態管理示範
  ├── App.vue               # 根組件
  └── main.ts              # 入口文件
```

## 授權

MIT
