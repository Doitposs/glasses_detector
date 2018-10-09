# Описание

Для распознавания человека в очках на фотографии предлагается использовать следующий подход.

## Модель

В качестве постановки задачи может рассматриваться бинарная классификация фото людей на предмет наличия очков или их отсутствия с использованием сверточной нейронной сети.

В качестве базовой модели можно использовать [модель](https://www.researchgate.net/publication/320964354_Shallow_convolutional_neural_network_for_eyeglasses_detection_in_facial_images) построенную на основе [GoogleNet](https://arxiv.org/abs/1409.4842). Однако, в нашем случае использовалась модель, построенная на основе [VGG16](https://arxiv.org/abs/1409.1556), но немного упрощенная из-за отсутствия gpu.

## Данные

Для распознования и выравнивания лиц на фото мы будем использовать готовый детектор из `dlib`.

В качестве датасета можно использовать [CelebA](http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html). Достаточно выбрать фото знаменитостей с очками и без. Выборка строилась как сбалансированная (~25000 фотографий). 

# Инструкции

1) В папке `models` запустите скрипт `get_shape_predictor.sh`, чтобы скачать модель для распознавания и выравнивания лица на фотографии.

**Для запуска модели на тестовой выборке перейдите к пункту 5**

2) Далее, создайте папку `data` в рабочей директории проекта (`mkdir data`). Загрузите в папку `data` два файла [`list_attr_celeba.txt`](https://drive.google.com/drive/folders/0B7EVK8r0v71pOC0wOVZlQnFfaGs) и 
[`img_aligh_celeba.zip`](https://drive.google.com/drive/folders/0B7EVK8r0v71pTUZsaXdaSnZBZzg). Распакуйте архив `unzip img_aligh_celeba.zip`.

3) Запустите файл `python3 prepare_data.py` для подготовки данных для обучения. Этот процесс может занять некоторое время. Его результатом будет выборка данных для обучения и тестирования `./data/images`.

4) Запустите обучение модели `python3 run_model.py`. Для получения результатов с использованием готовой модели запустите `python3 run_model.py --option validate`.

5) Для получения результатов на тестовой выборке запустите файл `python3 main.py path/to/images`. На вход подается директория с изображениями, на выходе - пути к файлам с людьми в очках в файле `results.txt`. Так же информация дублируется в поток вывода, кроме того ведется статистика работы алгоритма обнаружения лица, предоставляемый `dlib`.

# Возможные улучшения модели

Из возможных улучшений предлагается 

- добавить данных в тренировочную выборку (data augmentation etc.),

- настроить параметр затухания `learning rate` в оптимизаторе.

К сожалению, ограничения по времени расчетов, связанные с отсутствием gpu, не позволили проверить эти идеи.
