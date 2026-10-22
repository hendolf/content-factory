# Реестр интеграций и доказательств

Проверено 21.10.2026. Только официальные источники платформ и официальные коллекции Meta. Документация подтверждает наличие контракта, но не разрешения нашего приложения. В этом исследовании ни одна интеграция не получила статус «проверена на аккаунте».

Статусы: **D** — подтверждено прочитанным официальным источником; **G** — нужно пройти проверку аккаунта/приложения/тарифа; **U** — в проведённом исследовании не подтверждено, не обещать; **X** — исключено из MVP проектным решением. D и G могут стоять одновременно.

## Матрица

| Возможность | Статус | Условия / ограничение | Решение |
|---|---|---|---|
| Threads: текст, изображение, видео через создание контейнера и публикацию | D/G, S01–S02 | OAuth, разрешения приложения и пользователя; проверить формат на аккаунте | Кандидат первого адаптера после проверки |
| Threads: показатели поста | D/G, S03 | Есть endpoint insights; метрики и доступность проверять отдельно | Сохранять только реально возвращённые показатели |
| Threads: keyword search | D/G, S04 | В коллекции есть GET /keyword_search и scope threads_keyword_search; полнота выдачи и наши права не проверены | Отдельный исследовательский этап, ручные ссылки до проверки |
| Threads: ответы на публикации | D/G/X, S02 | Параметр reply_to_id подтверждает механизм ответа; это не разрешение на массовые действия | Отправку не реализовывать в MVP |
| Threads: полный список аудитории, контакты потенциальных покупателей, холодные DM | U/X | Из изученных источников не следует доступ к таким данным | Не закладывать в продукт |
| Instagram: публикация для Business/Creator | D/G, S05 | Поддерживаемые профессиональные аккаунты; выбранный Login определяет scopes и зависимости | Вначале готовые медиа и ручной экспорт; адаптер после проверки |
| Instagram: Facebook Login | D/G, S05 | Требует связанной Facebook Page; не работает с обычными consumer accounts | Отдельный путь авторизации, не смешивать со вторым |
| Instagram: Instagram Login | D/G, S06 | Отдельное семейство instagram_business_*; профессиональный аккаунт | Предпочтение выбрать после проверки сценария и разрешений |
| Instagram: insights | D/G, S05 | Общая возможность подтверждена; точный набор метрик, scopes и история для выбранного Login не зафиксированы | До реализации нужен реестр полей и реальные ответы |
| Instagram: входящие сообщения | D/G/X, S06 | В рассмотренном сценарии разговор начинает пользователь; наличие API не разрешает холодную рассылку | На пилоте перенос обращений вручную; inbox позднее |
| Instagram: всеобщий поиск людей, постов и произвольные внешние комментарии | U/X | Ограниченные возможности Facebook Login не равны универсальному поиску | Не обещать |
| Telegram: отправка текста и медиа в свой канал | D/G, S07 | Бот должен иметь нужные права, включая can_post_messages для канала | Основной кандидат первого прямого адаптера |
| Telegram: получение доступных обновлений и заявок боту | D/G, S07–S08 | Ограничено чатами, доступными боту; правила privacy mode | Входящий бот и webhook |
| Telegram: подробная статистика канала | D/G, S09 | Отдельный Telegram API/MTProto, доступность определяется can_view_stats; не обычный Bot API | В MVP ручной импорт с источником; MTProto не внедрять |
| Telegram: глобальный поиск чужих каналов/аудитории через Bot API | U/X | Методы бота не подтверждают такую возможность | Ручной список источников |
| TikTok: Direct Post | D/G, S10–S11 | Нужны approval video.publish, авторизация пользователя и аудит для снятия private-ограничений | Не критический путь MVP |
| TikTok: внутренний загрузчик только для команды | Ограничение D, S10 | Указан как неприемлемый intended use Direct Post | Не строить пилот на предположении, что он пройдёт аудит |
| TikTok: обязательные настройки публикации | D/G, S10, S12 | creator_info, выбор пользователем privacy, взаимодействий и согласия; требования к commercial disclosure | Сначала проект UX и проверка, затем код |
| TikTok: получение собственных публичных видео и счётчиков | D/G, S13–S14 | Display API, video.list и разрешённые поля; не весь TikTok и не полноценная бизнес-аналитика | Условный адаптер аналитики после доступа |
| TikTok: поиск всей аудитории, лидов, автоматические комментарии | U/X | Не подтверждено для предлагаемого коммерческого сценария | Не использовать Research API как обход |
| Metricool: планирование/изменение постов и экспорт данных | D/G, S15 | HTTP API зависит от тарифа и конкретной сети/формата | Пилотный посредник при подтверждённых правах |
| Metricool: API и MCP равнозначны | Неверное предположение, S16–S17 | Это разные способы доступа с разными условиями | Не превращать наличие инструмента в обещание фонового сервиса |
| Metricool: комментарии через MCP / комментарии Threads в Inbox API | Не поддержано описанным интерфейсом, S18 | В справке комментарии отсутствуют в MCP, Threads исключён из Inbox comments | Не использовать для автоматических комментариев |

## Контракты для проверки разработчиком

Это направления вызовов, а не готовая авторизованная интеграция. Версию API, scopes, лимиты и список полей фиксировать в артефакте CF-002/CF-009 перед кодом. Не брать токены и ID из примеров документации.

- Threads: POST /{threads-user-id}/threads → контейнер; POST /{threads-user-id}/threads_publish → ID публикации. GET /{threads-media-id}/insights. GET /keyword_search с q и search_type. Базовые кандидаты scopes: threads_basic, threads_content_publish, threads_manage_insights; для поиска threads_keyword_search. Режим доступа для внешних клиентов и требования review перепроверить отдельно.
- Instagram: создание media → проверка обработки → media_publish по выбранному Login. Не смешивать домены, токены и permission names между Facebook Login и Instagram Login. Полный сценарий OAuth, review и конкретные insights пока G.
- Telegram: getMe/getChat/getChatMember для проверки идентичности и прав; sendMessage/sendPhoto/sendVideo/sendMediaGroup по формату; webhook с secret_token; дедупликация update_id. Планирование — задача нашего сервера, не предположение о встроенном scheduler Bot API.
- TikTok: POST /v2/post/publish/creator_info/query/ перед Direct Post; инициализация публикации не равна окончательному успеху. Display API: POST /v2/video/list/, пагинация cursor/has_more. Нет оснований заменять им поиск чужих покупателей.
- Metricool: в справке API описаны X-Mc-Auth, userId и blogId; scheduler POST /api/v2/scheduler/posts. Конкретные network blocks и итоговый статус проверяются по Swagger и тесту выбранного формата. Не копировать универсальный пример Facebook в Threads/TikTok.

## Особые ограничения TikTok

Официальные Guidelines описывают ограничения unaudited-клиентов, аудит, intended use и обязательное участие пользователя в выборе настроек. Это делает обещание полностью фоновой автопубликации преждевременным. Даже будущая многоклиентская цель не гарантирует прохождения аудита. Допустимые маршруты пилота: публикация человеком в приложении либо провайдер с документированным поддерживаемым сценарием и проверкой нашего аккаунта. Наличие поля tiktokData у Metricool само по себе такой проверкой не является. [S10](https://developers.tiktok.com/docs/en/content-sharing-guidelines), [S11](https://developers.tiktok.com/docs/en/content-posting-api-get-started).

## Проверка аккаунта — обязательный протокол

1. Записать платформу, владельца, внешний account ID, тип аккаунта, регион, выбранный формат и путь интеграции.
2. Прочитать текущую документацию, условия использования, scopes, лимиты и условия review; сохранить дату и ссылки.
3. Авторизовать владельца штатным способом. Токены хранить в хранилище секретов; в отчёт — только факт наличия, scopes и срок.
4. Выполнить read-only вызов идентичности и прав. Сравнить результат с ожидаемым брендом.
5. На закрытом тестовом канале или после отдельного согласования конкретного материала выполнить минимальный тест публикации. Зафиксировать request ID, безопасный ответ, внешний ID, ссылку и фактический статус.
6. Проверить метрики отдельно, включая отсутствие данных. Обработать 401/403/429, истечение токена, отмену и неясный исход.
7. Присвоить capability статус verified либо blocked с причиной. Не включать функцию по одной лишь документации. Для внешнего клиента повторить условия доступа: успешный тест владельца приложения не доказывает доступ другим клиентам.

## Официальные источники

Ниже приведены ссылки и конкретный объём подтверждения. Источники не заменяют живую проверку.

- **S01** [Meta: официальный Threads workspace](https://www.postman.com/meta/threads/overview) и [вводная коллекции](https://www.postman.com/meta/threads/collection/dht3nzz/threads-api). Подтверждают происхождение коллекции, необходимость Meta App и пользовательской авторизации; предупреждают о возможной неполноте.
- **S02** [Meta Threads: создание и публикация](https://www.postman.com/meta/threads/documentation/dht3nzz/threads-api?entity=request-34203612-74fb48b1-ad1a-480e-b200-4dcdb8126a2f). Подтверждает контейнеры, publish и reply_to_id.
- **S03** [Meta Threads: Post Insights](https://www.postman.com/meta/threads/request/434u2bd/get-post-insights). Подтверждает endpoint, примеры metrics: views, likes, replies, reposts, quotes, shares; scopes в коллекции не равны выданным правам приложения.
- **S04** [Meta Threads: Search for Threads Posts](https://www.postman.com/meta/threads/request/34203612-b3b2c12a-7ce6-4d86-a3c6-6d31e3b66ea1). Подтверждает keyword_search, пример q/search_type=TOP; точные текущие ограничения требуют отдельной проверки.
- **S05** [Meta Instagram: официальная коллекция](https://www.postman.com/meta/instagram/documentation/6yqw8pt/instagram-api?entity=request-23987686-1ff01566-3509-48bd-a0f4-8571a91ccfdf). Подтверждает профессиональные аккаунты, publishing/insights и условия Facebook Login. Не является полным реестром метрик выбранного Login.
- **S06** [Meta Instagram: Instagram Login и messaging](https://www.postman.com/meta/instagram/documentation/6yqw8pt/instagram-api?entity=request-23987686-af579d08-121e-4897-8f45-5fd41ace49df). Подтверждает отдельные business scopes и входящий характер рассмотренного messaging-сценария.
- **S07** [Telegram Bot API](https://core.telegram.org/bots/api). Методы отправки, права администратора, updates, setWebhook и secret_token.
- **S08** [Telegram Bots FAQ](https://core.telegram.org/bots/faq). Какие сообщения получает бот и различие webhook/getUpdates.
- **S09** [Telegram: Channel statistics](https://core.telegram.org/api/stats). Отдельный API статистики, can_view_stats и stats_dc.
- **S10** [TikTok: Content Sharing Guidelines](https://developers.tiktok.com/docs/en/content-sharing-guidelines). Аудит, ограничения, intended use, обязательный UX и disclosures.
- **S11** [TikTok: Get Started — Direct Post](https://developers.tiktok.com/docs/en/content-posting-api-get-started). Продукт, video.publish, токен, проверка медиа и ограничения до аудита.
- **S12** [TikTok: Query Creator Info](https://developers.tiktok.com/docs/en/content-posting-api-reference-query-creator-info). Доступные privacy options и ограничения конкретного автора.
- **S13** [TikTok: List Videos v2](https://developers.tiktok.com/docs/en/tiktok-api-v2-video-list) и [Display API setup](https://developers.tiktok.com/docs/en/display-api-get-started). Список публичных видео авторизованного пользователя, scope video.list, необходимые approvals.
- **S14** [TikTok: Video Object](https://developers.tiktok.com/docs/en/tiktok-api-v2-video-object). Поля view_count, like_count, comment_count, share_count; они не дают контактов зрителей.
- **S15** [Metricool: Basic Guide for API Integration](https://help.metricool.com/basic-guide-for-api-integration-r97af). Авторизация, scheduler, draft и ограничения по сетям; использовать точную Swagger-схему при реализации.
- **S16** [Metricool: Plans and API access](https://help.metricool.com/plans-add-ons-and-api-access-explained-xux1u). API — Advanced/Custom, MCP доступен и на других планах; конкретный тариф пользователя не проверен.
- **S17** [Metricool: MCP vs API](https://help.metricool.com/mcp-vs-api-access-what-is-the-difference-5y3ib). Различие интерактивного инструмента и программной интеграции.
- **S18** [Metricool: API Access и comments](https://help.metricool.com/api-access-export-your-metricool-data-to-other-tools-and-automate-tasks-x8ln5). Ограничения Inbox comments и отсутствие comments через MCP.

Основные страницы Meta [Threads](https://developers.facebook.com/docs/threads/), [keyword search](https://developers.facebook.com/docs/threads/keyword-search/) и [Instagram Login](https://developers.facebook.com/docs/instagram-platform/instagram-api-with-instagram-login/) вернули 429. Их содержание не считается прочитанным. Нет оснований обещать исчерпывающую проверку всех возможностей платформ.
