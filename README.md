# Task-2
 Разработать скрипт на Ruby для командной строки.
 Принимает натуральное число параметров из командной строки
 Генерирует аккаунт пользователя на dev.by
 Формат записи логин-email-пароль
 
 Пример: Ruby devby.2b 3
 Вывод 3 логин-пароля, готовых к работе. Иначе вывод ошибки.
 
 


require 'rubygems'
require 'selenium-webdriver'
require 'capybara'
require 'securerandom'

def main
  #вызов основных функций
  numbers = 8
  login = generator(numbers)
  pass = generator(numbers)
  view("Login: " + login + "\n")
  view("Password:" + pass + "\n" + "----------------------")
  regInDevBy(login,pass)
  mailConfirm(login)

end

def generator(amountNumbersInSequense)
  #Создание через защищенный рандом рандомного хекса на зад. колво символов

  sequence = SecureRandom.hex(amountNumbersInSequense)
  return sequence

end


def view(msg)
  #вывод сообщений
  puts(msg)

end


def regInDevBy(login,pass)
  #регистрация на dev.by

  driver = Selenium::WebDriver.for :firefox
  driver.get 'https://dev.by/registration'
  driver.find_element(:name,"user[username]").send_keys login
  driver.find_element(:name,"user[email]").send_keys login+'@mailinator.com'
  driver.find_element(:name,"user[password]").send_keys pass
  driver.find_element(:name,"user[password_confirmation]").send_keys pass
  driver.find_element(:name,"user_agreement").click
  driver.find_element(:name,'commit').click
  driver.quit

end

def mailConfirm(login)
  #почта на малинаторе, подтверждение

  driver = Selenium::WebDriver.for :firefox
  driver.get 'https://www.mailinator.com'
  driver.find_element(:id,"inboxfield").send_keys login
  driver.find_element(:class_name,'input-group-btn').click
  sleep(10)
  driver.find_element(:class_name,'all_message-min').click
  sleep(3)
  driver.find_element(:id, "msg_body").click
  sleep(2)
  driver.action.send_keys(:tab).perform
  sleep(1)
  driver.action.send_keys(:enter).perform
  sleep(2)
  driver.quit

end


ARGV[0].times{main}

