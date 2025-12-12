import requests
import time
from concurrent.futures import ThreadPoolExecutor
import random

# Установим максимальное количество потоков для одновременных проверок
MAX_WORKERS = 10 

def check_website_status(url):
    """
    Проверяет статус доступности веб-сайта и возвращает структурированный результат.
    """
    timeout = 5 # Таймаут в секундах
    result = {'url': url, 'status': 'ОШИБКА: Неизвестно', 'latency': 0.0}
    
    try:
        start_time = time.time()
        # Выполняем GET-запрос с таймаутом
        response = requests.get(url, timeout=timeout)
        end_time = time.time()
        
        result['latency'] = (end_time - start_time) * 1000  # мс
        
        # Проверяем успешность (статус-код 200-299)
        if 200 <= response.status_code < 300:
            result['status'] = "ДОСТУПЕН (OK)"
        else:
            result['status'] = f"НЕДОСТУПЕН (HTTP {response.status_code})"
            
    except requests.exceptions.Timeout:
        result['status'] = f"ОШИБКА: Таймаут ({timeout} сек)"
    except requests.exceptions.ConnectionError:
        result['status'] = "ОШИБКА: Ошибка подключения"
    except requests.exceptions.RequestException as e:
        result['status'] = f"ОШИБКА: {type(e).__name__}"
        
    return result

def generate_sample_file(filename="websites_to_check_async.txt"):
    """Создает пример файла со списком URL для проверки."""
    sample_urls = [
        "https://www.google.com",
        "https://www.github.com",
        "https://www.stackoverflow.com",
        "https://www.python.org",
        "http://nosuchsite-345098.com", # Для проверки ошибки
        "https://www.wikipedia.org",
        "https://www.bing.com",
        "https://developer.mozilla.org",
    ]
    # Добавим несколько дубликатов для имитации нагрузки
    sample_urls.extend(random.sample(sample_urls, 3)) 
    
    with open(filename, 'w') as f:
        for url in sample_urls:
            f.write(f"{url}\n")
    print(f"✅ Создан файл со списком URL: '{filename}'")
    return filename

def run_concurrent_health_check(filename):
    """
    Читает URL, запускает проверку параллельно и выводит отчет.
    """
    try:
        with open(filename, 'r') as f:
            urls = [line.strip() for line in f if line.strip() and not line.startswith('#')]
    except FileNotFoundError:
        print(f"❌ ОШИБКА: Файл '{filename}' не найден.")
        return

    start_time_total = time.time()
    
    # Использование ThreadPoolExecutor для параллельного выполнения
    # 
    with ThreadPoolExecutor(max_workers=MAX_WORKERS) as executor:
        # map
