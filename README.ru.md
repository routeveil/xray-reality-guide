# Руководство по настройке Xray Reality VPN

![GitHub stars](https://img.shields.io/github/stars/routeveil/xray-reality-guide?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)
![Works in China](https://img.shields.io/badge/works%20in-China%20🇨🇳-red?style=flat-square)
![Works in Russia](https://img.shields.io/badge/works%20in-Russia%20🇷🇺-blue?style=flat-square)
![Protocol](https://img.shields.io/badge/protocol-Xray%20Reality-purple?style=flat-square)

Как поднять приватный VPN-сервер, который реально работает в России, Китае, Иране и Турции — на протоколе Xray Reality.

[English](README.md) · [中文](README.zh.md) · Русский

---

## Почему это нужно

Коммерческие VPN (ExpressVPN, NordVPN и другие) постоянно блокируются — потому что они используют одни IP-адреса для тысяч пользователей. Российские ТСПУ и китайский фаервол видят паттерны трафика и блокируют эти IP оптом.

С февраля 2026 года Россия заблокировала WireGuard, OpenVPN и стандартный IKEv2 на уровне протоколов по всей стране.

Xray Reality работает иначе. Вместо того чтобы оборачивать трафик в распознаваемый VPN-протокол, он выполняет настоящее TLS 1.3-рукопожатие с реальным легитимным доменом. Система блокировки проверяет соединение, видит обычный HTTPS-трафик — и пропускает его.

---

## Что понадобится

- VPS у любого крупного провайдера (~$5/месяц) — Hetzner, Vultr, Contabo
- «Прикрывающий» домен для TLS-рукопожатия (владеть им не нужно)
- Xray Core, установленный на сервере
- Клиентское приложение: Shadowrocket (iOS), v2rayNG (Android), Hiddify (Desktop)

---

## Почему это работает там, где WireGuard не работает

WireGuard использует UDP с характерным паттерном рукопожатия — Россия и Китай блокируют его за секунды. Reality выполняет настоящую TLS-сессию с легитимным доменом. Глубокая инспекция пакетов подтверждает, что трафик выглядит как обычный HTTPS — потому что технически так и есть.

📖 Полное техническое объяснение: [routeveil.com/blog/ru-xray-reality-vs-wireguard.html](https://routeveil.com/blog/ru-xray-reality-vs-wireguard.html)

---

## Рекомендуемые VPS-провайдеры

| Локация | Провайдер | Для кого |
|---|---|---|
| Франкфурт, Хельсинки | Hetzner | Россия, Турция, Иран |
| Токио, Сингапур | Vultr | Китай, низкая задержка |
| Нидерланды | Contabo | Бюджетный вариант |

---

## Быстрый старт

👉 [Полное руководство по настройке (SETUP.md)](SETUP.md)

👉 [Примеры конфигураций (CONFIG-EXAMPLES.md)](CONFIG-EXAMPLES.md)

---

## Не хотите настраивать самостоятельно?

Конфигурация требует правильного выбора прикрывающего домена, генерации ключей, согласования параметров клиента и сервера. Любая ошибка приводит к тому, что соединение либо не работает, либо становится обнаруживаемым.

**RouteVeil делает всё за вас** — разовая оплата $99, на вашем сервере, ваш IP, без ежемесячных платежей.

→ [routeveil.com](https://routeveil.com)  
→ Telegram: [t.me/routeveil](https://t.me/routeveil)

---

## По теме

- [Почему ваш VPN перестаёт работать каждые несколько месяцев](https://routeveil.com/blog/ru-v2rayn-ne-rabotaet-xray-reality.html)
- [Россия заблокировала Telegram. Потом рухнула банковская система.](https://routeveil.com/blog/ru-russia-telegram-vpn-bank.html)
- [Xray Reality vs WireGuard: почему один работает, а другой нет](https://routeveil.com/blog/ru-xray-reality-vs-wireguard.html)

---

## Ресурсы

- [Xray Core на GitHub](https://github.com/XTLS/Xray-core)
- [Официальная документация XTLS](https://xtls.github.io/)
