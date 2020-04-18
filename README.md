# Riding the Sine Line
A common task for RL: MountainCar-v0. A classic control problem: make 
an underpowered cart located at the bottom of the valley reach the flag. 

I solve it with Q-learning, track the progress and the solution space
by building nice maps and plots while and after training.

## Core ideas

Решение — нормальный такой дип ку-лёрнинг, в обучение добавлен некоторый читинг (ну или хакинг или шейпинг или трюки, если угодно).

![Карта: действие в зависимости от состояния](https://github.com/ivan-semyonich/tuda-syuda/blob/master/pics/actions_space_1586042498.png)

1. Чтобы агент побыстрее научился отползать с мёртвой точки и учился не целую вечность, 
**хвалим его за прирост в скорости**.
2. [**ОТКЛЮЧЕНО**] В начале обучения в историю мы сохраняем в том числе симметричные случаи 
(хоть задача и несимметрична, "раскачка" между холмами полезна).
3. В одном месте к признакам состояния добавляем их же, возведённые в квадрат (потому что если бы я писал эвристику, 
там бы были квадраты расстояния; так пусть машина сама подберёт что-то подобное). Ненужные члены полинома элиминируем 
гейтом, то есть линейным слоем + сигмоидой.
4. Близость до флажка решил не добавлять в обновлённый реворд, уж совсем читерство.

Пробовал что-то вроде Brain Damage, тупое усреднение, добавление шума в веса, добавление шума в данные (вообще оно-то 
как раз должно было помочь, потому что у позиции и скорости есть свойство локальности) — и ещё много что. Кое-что удалил, 
кое-что осталось в виде закомментированного кода (но я старался, чтобы было аккуратно).

Сохраняю раз в несколько шагов "карту" выученного поведения агента, а также историю ревордов. Довольно прикольно — 
и много рассказывает о задаче и пространствах решений.

## Howto

- точка входа — `main.py`
- архитектура агента и её обёртка — в `model.py`
- обновлённая награда — в `training.py`
- память (просто циклический буфер) — в  `memory.py`
- рисование картинок — в `utils.py`

![Ход обучения](https://github.com/ivan-semyonich/tuda-syuda/blob/master/pics/results_training_1586042492.png)

Бонусом пара неравенств, которые решают задачу, взяты с интернета 
и в виде готового кода  лежат в файле `deterministic_policy_for_comparison.py`.

![Решение на if-ах](https://github.com/ivan-semyonich/tuda-syuda/blob/master/pics/actions_space_DETERMINISTIC_POLICY.png)

Ещё один бонус — интересные и не очень картинки, которые получились в процессе.

![Что бывает, когда изменённое вознаграждение слишком далеко от исходного](https://github.com/ivan-semyonich/tuda-syuda/blob/master/pics/elephants_start_mating-when_you_pull_reward_shaping_levers_too_hard.png)

*Ситуация, когда вознаграждение агенту слишком сильно отличается от исходного. Сделать хотел утюг — слон получился вдруг.*

(Due to certain secrecy measures the code has originally been made available using a fake name: [Ivan Semyonich Golubtsov](https://github.com/ivan-semyonich/tuda-syuda))