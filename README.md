            const minutes = Math.floor((elapsedTime % 3600000) / 60000);
            const seconds = Math.floor((elapsedTime % 60000) / 1000);

            document.getElementById('timer').innerHTML = `${formatTime(hours)}:${formatTime(minutes)}:${formatTime(seconds)}`;
        }

        function formatTime(time) {
            return time < 10 ? `0${time}` : time;
        }

        function updateHistory() {
            const historyList = document.getElementById('historyList');
            historyList.innerHTML = '';
            
            workoutHistory.forEach((workoutRecord, index) => {
                const listItem = document.createElement('li');
                listItem.classList.add('workout-entry');

                const deleteBtn = document.createElement('button');
                deleteBtn.classList.add('delete-btn');
                deleteBtn.textContent = 'Delete';
                deleteBtn.onclick = () => deleteWorkoutEntry(index);

                listItem.innerHTML = workoutRecord.replace(/(\b\w)/g, char => char.toUpperCase());
                listItem.appendChild(deleteBtn);

                historyList.appendChild(listItem);
            });
        }

        function deleteWorkoutEntry(index) {
            workoutHistory.splice(index, 1);

            // Save the updated workout history to localStorage
            localStorage.setItem('workoutHistory', JSON.stringify(workoutHistory));

            updateHistory();
        }

        function calculateWorkoutTime() {
            const currentTime = new Date();
            const elapsedTime = (currentTime - startTime) / 1000;
            const hours = Math.floor(elapsedTime / 3600);
            const minutes = Math.floor((elapsedTime % 3600) / 60);
            const seconds = Math.floor(elapsedTime % 60);

            return `${formatTime(hours)}:${formatTime(minutes)}:${formatTime(seconds)}`;
        }

        function showExerciseOptions() {
            const exerciseCategory = document.getElementById('exerciseCategory').value;
            const exerciseOptionsDiv = document.getElementById('exerciseOptions');
            const specificExerciseSelect = document.getElementById('specificExercise');
            const chestOptions = ['Bench Press', 'Incline Bench Press', 'Incline Dumbbell Bench Press', 'Dumbbell Bench Press', 'Chest Flies'];
            const tricepsOptions = ['Tricep Rope Pushdown', 'Dips', 'Close Grip Bench Press', 'Rope Extensions'];
            const shouldersOptions = ['Shoulder Press', 'Lateral Raises', 'Shoulder Shrugs', 'Dumbbell Front Lateral Raises'];
            const backOptions = ['Rows', 'Deadlift', 'Lat Pulldown', 'Cable Row', 'T-Bar Row'];
            const bicepsOptions = ['Hammer Curls', 'V-Bar Curls', 'Dumbbell Curls'];

            switch (exerciseCategory) {
                case 'chest':
                    fillOptions(specificExerciseSelect, chestOptions);
                    break;
                case 'triceps':
                    fillOptions(specificExerciseSelect, tricepsOptions);
                    break;
                case 'shoulders':
                    fillOptions(specificExerciseSelect, shouldersOptions);
                    break;
                case 'back':
                    fillOptions(specificExerciseSelect, backOptions);
                    break;
                case 'biceps':
                    fillOptions(specificExerciseSelect, bicepsOptions);
                    break;
                default:
                    break;
            }

            exerciseOptionsDiv.style.display = 'block';
        }

        function fillOptions(selectElement, options) {
            selectElement.innerHTML = '';
            options.forEach(option => {
                const formattedOption = option.replace(/([A-Z])/g, ' $1').trim();
                const optionElement = document.createElement('option');
                optionElement.value = option;
                optionElement.textContent = formattedOption;
                selectElement.appendChild(optionElement);
            });
        }

        function logExercise() {
            const exerciseCategory = document.getElementById('exerciseCategory').value;
            const specificExercise = document.getElementById('specificExercise').value;
            const reps = document.getElementById('reps').value;
            const weight = document.getElementById('weight').value;
            const logDate = new Date().toLocaleDateString();
            const logEntry = `${exerciseCategory} - ${specificExercise} - Reps: ${reps}, Weight: ${weight} lbs`;

            exerciseLog.unshift({ date: logDate, entry: logEntry }); // Add to the beginning of the array

            // Save the exercise log to localStorage
            localStorage.setItem('exerciseLog', JSON.stringify(exerciseLog));

            // Display the exercise log
            updateExerciseLog();
        }

        function deleteExerciseEntry(index) {
            exerciseLog.splice(index, 1);

            // Save the updated exercise log to localStorage
            localStorage.setItem('exerciseLog', JSON.stringify(exerciseLog));

            updateExerciseLog();
        }

        function updateExerciseLog() {
            const logSection = document.getElementById('exerciseLog');
            logSection.innerHTML = '';

            let currentDate = null;

            exerciseLog.forEach((log, index) => {
                if (log.date !== currentDate) {
                    const dateHeading = document.createElement('div');
                    dateHeading.classList.add('log-date');
                    dateHeading.textContent = log.date;
                    logSection.insertBefore(dateHeading, logSection.firstChild); // Insert at the beginning
                    currentDate = log.date;
                }

                const listItem = document.createElement('div');
                listItem.classList.add('log-entry');

                const deleteBtn = document.createElement('button');
                deleteBtn.classList.add('delete-btn');
                deleteBtn.textContent = 'Delete';
                deleteBtn.onclick = () => deleteExerciseEntry(index);

                listItem.innerHTML = log.entry.replace(/(\b\w)/g, char => char.toUpperCase());
                listItem.appendChild(deleteBtn);

                logSection.insertBefore(listItem, logSection.firstChild); // Insert at the beginning
            });
        }

        function logWeight() {
            const weightInput = document.getElementById('weightInput').value;
            const weightDate = new Date().toLocaleDateString();
            const weightEntry = `${weightDate} - Weight: ${weightInput} lbs`;

            weightLog.unshift(weightEntry); // Add to the beginning of the array

            // Save the weight log to localStorage
            localStorage.setItem('weightLog', JSON.stringify(weightLog));

            // Display the weight log
            updateWeightLog();
        }

        function updateWeightLog() {
            const weightLogSection = document.getElementById('weightLog');
            weightLogSection.innerHTML = '';

            weightLog.forEach(weightEntry => {
                const listItem = document.createElement('div');
                listItem.classList.add('weight-entry');

                const deleteBtn = document.createElement('button');
                deleteBtn.classList.add('delete-btn');
                deleteBtn.textContent = 'Delete';
                deleteBtn.onclick = () => deleteWeightEntry(weightLog.indexOf(weightEntry));

                listItem.innerHTML = weightEntry.replace(/(\b\w)/g, char => char.toUpperCase());
                listItem.appendChild(deleteBtn);

                weightLogSection.appendChild(listItem);
            });
        }

        function deleteWeightEntry(index) {
            weightLog.splice(index, 1);

            // Save the updated weight log to localStorage
            localStorage.setItem('weightLog', JSON.stringify(weightLog));

            updateWeightLog();
        }
    </script>
</body>
</html>
