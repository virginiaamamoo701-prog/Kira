import requests
import time
import argparse
import logging
from concurrent.futures import ThreadPoolExecutor

# --- Конфигурация Логирования ---
LOG_FILENAME = 'health_check_report.log'
logging.basicConfig(
    filename=LOG_FILENAME,
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s'
)
# --- Конец Конфигурации ---

def check_website_status(url, timeout):
    """Проверяет статус доступности веб-сайта."""
    result = {'url': url, 'status': 'ОШИБКА: Неизвестно', 'latency': 0.0}
    
    try:
        start_time = time.time()
        response = requests.get(url, timeout=timeout)
        end_time = time.time()
        
        result['latency'] = (end_time - start_time) * 1000  # мс
        
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

def run_concurrent_health_check(filename, max_workers, timeout):
    """Читает URL, запускает проверку параллельно и выводит/логирует отчет."""
    try:
        with open(filename, 'r') as f:
            urls = [line.strip() for line in f if line.strip() and not line.startswith('#')]
    except FileNotFoundError:
        error_msg = f"ОШИБКА: Файл списка URL '{filename}' не найден."
        print(f"❌ {error_msg}")
        logging.error(error_msg)
        return

    start_time_total = time.time()
    
    print(f"\n--- 📈 УСОВЕРШЕНСТВОВАННЫЙ Health Check ---")
    print(f"Файл: {filename} | Потоков: {max_workers} | Таймаут: {timeout} с")
    print("-" * 85)

    with ThreadPoolExecutor(max_workers=max_workers) as executor:
        results = list(executor.map(lambda url: check_website_status(url, timeout), urls))
        
    end_time_total = time.time()

    # --- Формирование отчета ---
    
    # Заголовок для консоли
    report_header = f"| {'URL':<45} | {'СТАТУС':<20} | {'ВРЕМЯ ОТВЕТА (мс)':<15} |"
    print(report_header)
    print("-" * 85)
    logging.info(f"Начало проверки. Проверено {len(urls)} сервисов за {end_time_total - start_time_total:.2f} сек.")
    
    for result in results:
        latency_str = f"{result['latency']:.2f}" if result['latency'] > 0.0 else "N/A"
        
        # Вывод в консоль
        print(f"| {result['url']:<45} | {result['status']:<20} | {latency_str:<15} |")
        
        # Запись в лог-файл
        log_message = f"URL: {result['url']}, Status: {result['status']}, Latency: {latency_str}"
        if "ОШИБКА" in result['status'] or "НЕДОСТУПЕН" in result['status']:
            logging.warning(log_message)
        else:
            logging.info(log_message)
        
    print("-" * 85)
    print(f"Проверка завершена. Детальный отчет сохранен в '{LOG_FILENAME}'")
    
# --- Настройка аргументов командной строки (
