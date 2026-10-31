# Лабораторная № 1: качество вина

Решение: `regression/lab01.ipynb`. Ноутбук содержит выполненные ячейки, графики и объяснения на русском. Используются оба вида вина UCI Wine Quality и признак `is_red` (1 — красное, 0 — белое).

## Запуск на этом компьютере

Откройте терминал в папке репозитория:

```bash
source .venv/bin/activate
jupyter lab regression/lab01.ipynb
```

Выберите ядро **Python (ML lab 1)** и команду **Restart Kernel and Run All Cells**. Подбор моделей занимает несколько минут. Результаты уже сохранены, поэтому для просмотра запускать обучение заново не обязательно.

## Восстановление окружения

Проверено на Python 3.12.14. Из корня репозитория:

```bash
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m ipykernel install --prefix .venv --name ml-lab01 --display-name "Python (ML lab 1)"
jupyter lab regression/lab01.ipynb
```

Если работа идёт в VS Code, выберите интерпретатор `.venv/bin/python`. Исходные CSV включены в репозиторий: после установки библиотек интернет для запуска не нужен. Пути к CSV работают из корня репозитория и из папки `regression`.

## Полученные результаты

Пять групповых фолдов, `random_state=42`; train — 5222, test — 1275 строк. Победитель выбран по CV до просмотра test.

| Модель | CV RMSE | Test RMSE | Test MAE |
|---|---:|---:|---:|
| Baseline | 0.8674 | 0.8968 | 0.7072 |
| Linear Regression | 0.7348 | 0.7348 | 0.5639 |
| Random Forest | **0.6858** | **0.6965** | **0.5308** |
| Gradient Boosting | 0.6972 | 0.7083 | 0.5455 |

Лес: 300 деревьев, `max_depth=None`, `min_samples_leaf=1`. Локальный порог RMSE < 0.70 достигнут с небольшим запасом; скрытый тест недоступен. На редких оценках ошибки выше, а Train RMSE леса 0.2161 заметно ниже Test RMSE: эти ограничения разобраны в ноутбуке.

## Проверка эксперимента

- Основная метрика — RMSE; дополнительная — MAE.
- Около 20% строк отложено для финального теста; одинаковые признаки находятся в одной группе.
- Подбор настроек проводится только по пятифолдовой групповой кросс-валидации train.
- Победитель определяется до просмотра test. Все четыре решения, включая baseline, проверены на одинаковой тестовой части.
- Локальный результат не является результатом на скрытом тесте преподавателя.

## Данные

Cortez et al. (2009), [UCI Wine Quality](https://archive.ics.uci.edu/dataset/186/wine+quality), [DOI: 10.24432/C56S3T](https://doi.org/10.24432/C56S3T), CC BY 4.0.

Файлы `regression/data/winequality-red.csv`, `winequality-white.csv` и описание `winequality.names` взяты из официального [архива UCI](https://archive.ics.uci.edu/static/public/186/wine+quality.zip) без изменений. Метаданные и контрольные суммы — в `regression/data/SOURCE.md`.

Исходные материалы семинара и задания не менялись. Работа локальная: коммит и отправка на GitHub не выполнялись.
