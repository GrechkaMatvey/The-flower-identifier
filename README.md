# The-flower-identifier
Определение видов растений вручную может быть сложным, особенно для людей, не обладающих специальными знаниями. Этот проект предлагает инструмент на основе нейросети, который классифицирует растения по их изображениям, ускоряя процесс идентификации и минимизируя ошибки.  
![123321421](https://github.com/user-attachments/assets/753a2cec-ca30-4b68-a81e-eed3ed2cb7c6)

##Содержание
1. Выбор темы
Идея:Создать приложение, которое поможет пользователям (садоводам, ботаникам, любителям природы) быстро определять вид растения, используя фотографию его листьев, цветов или стебля.
В проекте используется обученная модель сервиса Google Teachablem Machine
2. Постановка задачи
Задача:  
Создать модель, которая классифицирует фотографии растений на 10-40 наиболее распространённых видов.  
Решение:  
- Собрать изображения растений для обучения модели.  
- Обучить GTM для классификации этих изображений.
3. Выбор инструментов  
- Платформы:  
  - Google Teachable Machine 
  - GitHub – для хранения кода и документации.
  - Браузер: Chrome, Edge, Safari
 4. Сбор данных  
Источники данных:  
- Открытые датасеты:  
  - PlantCLEF Dataset – набор данных для классификации растений.  
  - Kaggle Plant Species Dataset.
  - GitHub DataSet Oxford flowers 102
Предварительная обработка:  
1. Сбор данных по видам растений.  
2. Аугментация данных (повороты, отражение, изменение яркости) для увеличения объёма тренировочных данных.  
3. Приведение изображений к единому размеру (например, 128x128 пикселей).

## Использование модели
1. Используйте встроенную функцию Teachable Machine для классификации растений
2. Протестируйте модель, загружая изображения разных растений, которых она раньше не видела
   Результаты: Точность модели 95% на тестовом наборе данных
![Ytqhjre](https://github.com/user-attachments/assets/dedaf9b3-85dd-4601-a83b-934ada74efe6)


```java
<div>Teachable Machine Image Model</div>
<button type="button" onclick="init()">Start</button>
<div id="webcam-container"></div>
<div id="label-container"></div>
<script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@latest/dist/tf.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@teachablemachine/image@latest/dist/teachablemachine-image.min.js"></script>
<script type="text/javascript">
    // More API functions here:
    // https://github.com/googlecreativelab/teachablemachine-community/tree/master/libraries/image

    // the link to your model provided by Teachable Machine export panel
    const URL = "https://teachablemachine.withgoogle.com/models/leJKoHZY0/";

    let model, webcam, labelContainer, maxPredictions;

    // Load the image model and setup the webcam
    async function init() {
        const modelURL = URL + "model.json";
        const metadataURL = URL + "metadata.json";

        // load the model and metadata
        // Refer to tmImage.loadFromFiles() in the API to support files from a file picker
        // or files from your local hard drive
        // Note: the pose library adds "tmImage" object to your window (window.tmImage)
        model = await tmImage.load(modelURL, metadataURL);
        maxPredictions = model.getTotalClasses();

        // Convenience function to setup a webcam
        const flip = true; // whether to flip the webcam
        webcam = new tmImage.Webcam(200, 200, flip); // width, height, flip
        await webcam.setup(); // request access to the webcam
        await webcam.play();
        window.requestAnimationFrame(loop);

        // append elements to the DOM
        document.getElementById("webcam-container").appendChild(webcam.canvas);
        labelContainer = document.getElementById("label-container");
        for (let i = 0; i < maxPredictions; i++) { // and class labels
            labelContainer.appendChild(document.createElement("div"));
        }
    }

    async function loop() {
        webcam.update(); // update the webcam frame
        await predict();
        window.requestAnimationFrame(loop);
    }

    // run the webcam image through the image model
    async function predict() {
        // predict can take in an image, video or canvas html element
        const prediction = await model.predict(webcam.canvas);
        for (let i = 0; i < maxPredictions; i++) {
            const classPrediction =
                prediction[i].className + ": " + prediction[i].probability.toFixed(2);
            labelContainer.childNodes[i].innerHTML = classPrediction;
        }
    }
</script>
```

[Веб-версия для определения](https://teachablemachine.withgoogle.com/models/leJKoHZY0/)
