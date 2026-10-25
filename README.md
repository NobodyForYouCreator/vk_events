# Сбор и семантическое обогащение мест (POI) для рекомендаций

Тестовое задание на курсовую работу: пайплайн сбора ~100 мест Москвы, извлечение семантических признаков, EDA и семантический поиск.

**Всё решение находится в одном ноутбуке: [`poi_pipeline.ipynb`](poi_pipeline.ipynb)** (задания 1–5, код, пояснения, графики и выводы).

## Структура

```
poi_pipeline.ipynb              ноутбук с решением (выполнен, с выводами)
data/raw/                       кеш ответов API (OSM, Wikivoyage, Wikidata, Википедия): ноутбук перезапускается без сети
data/places.csv / .json         датасет мест (задание 1)
data/places_features.csv / .json  датасет с извлечёнными признаками (задания 2–3)
results/feature_extraction_comparison.csv   сравнение методов извлечения признаков на ручной разметке
results/search_comparison.xlsx / .csv       сравнительная таблица поиска (задание 5)
results/search_report.md        сравнительная таблица и выводы по поиску (задание 5)
results/figures/                графики EDA и поиска
```

## Кратко о результатах

| | |
|---|---|
| Датасет | 100 мест, 10 категорий (квоты 6–12), Москва; источники: OpenStreetMap, Википедия (+ просмотры), Wikidata, Wikivoyage |
| Признаки | тип опыта, атмосфера, контекст, бюджет, время, популярность, помещение/улица, длительность, Wi-Fi/веранда/вегетарианское/доступность |
| Извлечение | правила (теги, часы, цены, словари) + zero-shot на `multilingual-e5-base`; гибрид macro F1 = 0.71 на 30 размеченных местах |
| Поиск | nDCG@5: BM25 0.44 → dense 0.58 → dense + признаки в тексте 0.67 → гибрид с бонусами за признаки 0.77 |

## Запуск

Локально:
```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook poi_pipeline.ipynb
```

Colab:
```
!git clone https://git.miem.hse.ru/<user>/poi-recsys-test.git
%cd poi-recsys-test
!pip install -q -r requirements.txt
```
Модели (`intfloat/multilingual-e5-base`, `MoritzLaurer/multilingual-MiniLMv2-L6-mnli-xnli`) скачаются с HuggingFace при первом запуске. Полный прогон на CPU занимает ~4–5 минут.
