# How Fine-Tuning Qwen
## Файлы
- [Скрипт создания датасета](create_dataset.ipynb)
- [Скрипт обучения](train_model.ipynb)

## Создание датасета
При созданье датасета создастся создаться два файла

- dataset_info.json
- data/{timestamp}.json

В dataset_info добавится созданный датасет с ключом `task_description` и полным путём до файла.

## Обучение модели

Указываем нужную нам модель и датасет

```
model="Qwen/Qwen2.5-7B-Instruct"
task_description = "recalculate"
```

Указываем параметры обучения

```
step = 500
batch_size = 20
max_prompt_size = 10000
lora_rank = 16
lora_alpha = 32
lora_dropout = 0
save_steps = 5000
```

Запускаем обучение

## Запуск модели
После обучения появится файл с LoRA весами `lora/{model}/{task_description}/train_{timestamp}`

Пример запуска
```commandline
vllm serve Qwen/Qwen2.5-7B-Instruct --enable_prefix_caching --max_model_len=12000 --disable-log-requests \
     --enable-lora \
     --lora-modules '{"name": "Qwen-recalculate", "path": "lora/Qwen/Qwen2.5-7B-Instruct/recalculate/train_2025-01-25-11-56-28", "base_model_name": "Qwen/Qwen2.5-7B-Instruct"}'\
     --quantization fp8
```