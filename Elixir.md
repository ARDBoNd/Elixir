![Elixir](https://github.com/elixir-lang/elixir-lang.github.com/raw/master/images/logo/logo.png)
=========


Elixir - это динамический, функциональный язык, разработанный для создания масштабируемых и поддерживаемых приложений.
В Elixir используется виртуальная машина Erlang, известная своими низкоуровневыми распределенными и отказоустойчивыми системами, а также успешно используемая в веб-разработке и в области встроенного программного обеспечения.


# Особенности платформы
## Масштабируемость
Весь код Elixir выполняется внутри облегченных потоков выполнения (называемых процессами), которые изолированы и обмениваются информацией посредством сообщений:
```
current_process = self()

# Spawn an Elixir process (not an operating system one!)
spawn_link(fn ->
  send(current_process, {:msg, "hello world"})
end)

# Block until the message is received
receive do
  {:msg, contents} -> IO.puts(contents)
end
```
Из-за их легкого характера, нередки случаи, когда на одной машине одновременно работают сотни тысяч процессов. Изоляция позволяет независимо собирать процессы, сокращая паузы в системе и максимально эффективно используя все ресурсы машины (вертикальное масштабирование).

Процессы также могут связываться с другими процессами, работающими на разных машинах в одной сети. Это обеспечивает основу для распространения, позволяя разработчикам координировать работу между несколькими узлами (горизонтальное масштабирование).

## Отказоустойчивость
Неизбежная правда о программном обеспечении, работающем в производстве, состоит в том, что все пойдет не так. Еще больше, если учесть сеть, файловые системы и другие сторонние ресурсы.

Чтобы справиться со сбоями, Elixir предлагает супервизоры, которые описывают, как перезапускать части вашей системы, когда что-то идет не так, возвращаясь к известному начальному состоянию, которое гарантированно будет работать:
```
children = [
  TCP.Pool,
  {TCP.Acceptor, port: 4040}
]

Supervisor.start_link(children, strategy: :one_for_one
```
Комбинация масштабируемости, отказоустойчивости и событийного программирования посредством передачи сообщений делает Elixir отличным выбором для реактивного программирования и архитектур.

## Особенности языка
Функциональное программирование продвигает стиль кодирования, который помогает разработчикам писать короткий, лаконичный и понятный код. Например, сопоставление с образцом позволяет разработчикам легко деструктурировать данные и получать доступ к их содержимому:
```
%User{name: name, age: age} = User.get("John Doe")
name #=> "John Doe"
```
В сочетании с защитой сопоставление с образцом позволяет нам элегантно сопоставлять и утверждать конкретные условия для выполнения некоторого кода:
```
def drive(%User{age: age}) when age >= 16 do
  # Code that drives a car
end

drive(User.get("John Doe"))
#=> Fails if the user is under 16
```

Elixir в значительной степени полагается на эти функции, чтобы обеспечить работу вашего программного обеспечения в ожидаемых условиях. 
## Расширяемость и DSLs

Elixir был разработан для расширения, позволяя разработчикам естественным образом расширять язык для определенных доменов, чтобы повысить их производительность.

В качестве примера давайте рассмотрим простой тестовый пример с использованием тестовой среды Elixir под названием ExUnit:
```
defmodule MathTest do
  use ExUnit.Case, async: true

  test "can add two numbers" do
    assert 1 + 1 == 2
  end
end
```
Опция `async: true` позволяет выполнять `тесты` параллельно, используя как можно больше ядер ЦП, а функциональность assert может анализировать ваш код, предоставляя отличные отчеты в случае сбоев. Эти функции создаются с помощью макросов Elixir, что позволяет добавлять новые конструкции, как если бы они были частью самого языка.
# Ссылки
* Сайт разработчика https://elixir-lang.org;
* Github https://github.com/elixir-lang 
* Страница в векипедии https://ru.wikipedia.org/wiki/Elixir_;
