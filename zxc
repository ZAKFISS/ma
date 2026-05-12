import serial
import subprocess
import time

# --- НАСТРОЙКИ ---
SERIAL_PORT = '/dev/ttyUSB0'  # Узнайте ваш порт командой: ls /dev/ttyUSB* или /dev/ttyACM*
BAUD_RATE = 9600
SOUND_FILE = '/home/user/Music/notification.wav' # Замените на путь к вашему файлу
# -----------------

def play_sound(file_path):
    """Воспроизводит аудиофайл, используя системный проигрыватель."""
    try:
        # Используем 'aplay' для WAV или 'mpg123'/'mpv' для MP3
        # Убедитесь, что нужный проигрыватель установлен в вашей системе
        subprocess.run(['aplay', '-q', file_path], check=True) 
        print(f"Звук воспроизведен: {file_path}")
    except Exception as e:
        print(f"Ошибка воспроизведения: {e}")
        # Если aplay не сработал, попробуем mpv (для mp3 и других форматов)
        try:
            subprocess.run(['mpv', '--no-terminal', file_path], check=True)
            print(f"Звук воспроизведен с помощью mpv: {file_path}")
        except Exception as e2:
            print(f"Не удалось воспроизвести звук и через mpv: {e2}")

if __name__ == "__main__":
    try:
        ser = serial.Serial(SERIAL_PORT, BAUD_RATE, timeout=1)
        print(f"Слушаю порт {SERIAL_PORT}...")
        time.sleep(2)  # Даем время на установку соединения
        ser.flushInput()
        
        while True:
            line = ser.readline().decode('utf-8').strip()
            if line == "PLAY":
                print("Сигнал нажатия кнопки получен!")
                play_sound(SOUND_FILE)
    except serial.SerialException:
        print(f"Ошибка: не могу открыть порт {SERIAL_PORT}. Проверьте подключение и права доступа.")
    except KeyboardInterrupt:
        print("Программа остановлена пользователем.")
    finally:
        if 'ser' in locals() and ser.is_open:
            ser.close()