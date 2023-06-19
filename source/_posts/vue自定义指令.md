---
title: vue自定义指令
date: 2023-06-19 11:15:00
tags:
---

```js
// main.ts 引入挂载
import { loadDirectives } from "@/directives"
/** 加载自定义指令 */
loadDirectives(app)


文件夹结构：
directives{
    permission->index.ts
    index.ts
}

// 外层index.ts挂载
import { type App } from "vue";
import { permission, btnpermission, repeatClick } from "./permission";
/** 挂载自定义指令 */
export function loadDirectives(app: App) {
  app.directive("permission", permission);
  app.directive("btnpermission", btnpermission);
  app.directive("repeatClick", repeatClick);
}

// 自定义指令事件  permission->index.ts
/** 权限指令使按钮显示隐藏 */
import { type Directive } from "vue";
import { useBrandStore } from "@/store/modules/brand";
import { ElMessage } from "element-plus";
export const permission: Directive = {
  mounted(el, binding) {
    const { value } = binding;
    const roles = useBrandStore().venue.power;
    // const roles = useUserStoreHook().roles
    if (value && value instanceof Array && value.length > 0) {
      const permissionRoles = value;
      const hasPermission = roles.some((role: any) => {
        return permissionRoles.includes(role);
      });
      if (!hasPermission) {
        el.style.display = "none";
      }
    } else {
      throw new Error(`need roles! Like v-permission="['admin','editor']"`);
    }
  },
};

// 按钮根据权限点击拦截 v-btnpermission:admin="click"
export const btnpermission: Directive = {
  mounted(el, binding) {
    // const { value } = binding
    el.addEventListener("click", customClick(binding))
  }
}
const customClick = (binding: any) => {
  return () => {
    const roles = useBrandStore().venue.power
    if (roles.includes(binding.arg)) {
      // 判断条件
      // 正常触发点击事件
      binding.value()
    } else {
      // 已经成功拦截点击事件
      ElMessage.error("您没有权限")
    }
  }
}

// 按钮几秒内只能点击一次 v-repeatClick="1000"
export const repeatClick: Directive = {
  mounted(el, binding) {
    el.addEventListener("click", () => {
      const events = el.style.pointerEvents
      if (events == "") {
        el.style.pointerEvents = "none"
        setTimeout(() => {
          el.style.pointerEvents = ""
        }, binding.value || 1000)
      }
    })
  }
}
```
