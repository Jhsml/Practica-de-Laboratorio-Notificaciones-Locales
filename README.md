# Aviso laboratorio · Notificaciones locales

**Estudiante:** Villa Gomez, Geysen Jhosmel – 6003125431
**Curso:** Programación de Aplicaciones Móviles – VI ciclo

App en React Native + Expo (blank-typescript) que, al tocar un botón, programa una notificación local con título y la fecha y hora en que se programó.

```bash
npm install
npx expo start
```

## Respuestas de la comprobación final

**1. ¿En qué archivo se comprueba el permiso?**
En `src/notifications/localNotifications.ts`, en la función `ensureNotificationPermission()`. Primero consulta con `getPermissionsAsync()` y, si no hay permiso, lo pide con `requestPermissionsAsync()`. En Android crea antes el canal `laboratorio`.

**2. ¿En qué archivo se vincula la acción al botón?**
En `App.tsx`: el `Pressable` tiene `onPress={handleNotify}`. `handleNotify` pide el permiso y, si se concede, llama a `showLocalNotification()`.

**3. ¿Por qué es una notificación local aunque aparezca en la misma bandeja que las push?**
Porque la genera el propio teléfono a pedido de la app (`scheduleNotificationAsync` con un trigger de 3 segundos), sin ningún servidor. Una push la envía un servidor a través de un servicio de mensajería. Ambas se muestran en la misma bandeja del sistema, pero su origen es distinto.

---

## ⚠️ Inconveniente encontrado

> **En Android con Expo Go 58.0.5 (la última versión disponible), crear el canal falla con un error nativo:**
>
> ```
> ExpoNotificationChannelManager.setNotificationChannelAsync has been rejected
> → java.lang.NullPointerException: ... getClass() on a null object reference
> ```
>
> El código del paso 4 es correcto según la documentación de Expo; el fallo es de Expo Go y está reportado en
> [expo/expo#53907](https://github.com/expo/expo/issues/53907) (también ocurrió en SDK 57).
> Con la guía tal cual, la app mostraba "No se pudo preparar la notificación".

**Solución aplicada (único cambio respecto a la guía):** en `ensureNotificationPermission()` se envolvió `setNotificationChannelAsync` en un `try/catch`. Si el canal no se puede crear, la app continúa y Android usa el canal por defecto de `expo-notifications`. Con esto la notificación aparece correctamente en la bandeja.


