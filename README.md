import profile
import time
import pyautogui
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.firefox.service import Service
from selenium.webdriver.firefox.options import Options
from selenium.webdriver.common.by import By
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import TimeoutException
import os
import shutil


options = Options()


service = Service(r'C:\Users\rcasetta01\Desktop\Nova pasta\Bot\geckodriver.exe')
# service = Service(r'C:\Users\User\Desktop\botRafa\geckodriver.exe')

profile = webdriver.FirefoxProfile()


profile.set_preference("browser.download.folderList", 2)
profile.set_preference("browser.download.useDownloadDir", False) 
profile.set_preference("browser.download.manager.showWhenStarting", True) 


profile.set_preference("browser.helperApps.neverAsk.saveToDisk", "") 


options.profile = profile


driver = webdriver.Firefox(service=service, options=options)



def presence_and_clickable_and_visible(locator):
    def condition(driver):
        try:
            element = EC.visibility_of_element_located(locator)(driver)
            return element and element.is_displayed() and EC.element_to_be_clickable(locator)(driver)
        except TimeoutException:
            return False
    return condition


print("Bem-Vinda, Daiane! :")


driver.get('https://app.remini.ai/?utm_source=reminiai&utm_medium=try-remini')



#Coloca em tela cheia
#pyautogui.press('f11')
pyautogui.hotkey('win', 'up')
time.sleep(2)



#pyautogui.keyDown('ctrl')

# Realiza o scroll do mouse para baixo para diminuir o zoom
#for _ in range(3):
    #pyautogui.scroll(-100)

# Solta a tecla Ctrl
#pyautogui.keyUp('ctrl')


wait = WebDriverWait(driver, 10)
accept_button = wait.until(EC.element_to_be_clickable((By.ID, "onetrust-accept-btn-handler")))
accept_button.click()
print("Cookies aceitos.")


########################################################Login
wait = WebDriverWait(driver, 10)
login_button = wait.until(EC.element_to_be_clickable((By.CSS_SELECTOR, 'button[data-cy="login-btn"]')))
login_button.click()

########################################################Fonte login
wait = WebDriverWait(driver, 10)
google_button = wait.until(EC.element_to_be_clickable((By.CSS_SELECTOR, 'button[data-cy="login-provider-btn"] img[src*="google"]')))
google_button.click()

print("Por favor, faça o Login...")



########################################################Mostrar o tempo restante enquanto aguarda
tempo_total = 40
for segundos_restantes in range(tempo_total, 0, -1):
    print(f"Tempo restante: {segundos_restantes} segundos", end='\r')
    time.sleep(1)




########################################################Primeiro botão de Upload
upload_button = wait.until(EC.visibility_of_element_located((By.XPATH, "//div[@class='btn btn--large btn--primary w-max mx-auto py-1 px-6 ps-8 flex justify-center items-center rounded-full']")))
upload_button.click()
print(' ' * 30, end='\r')
print("Iniciando uploads...")




########################################################Escolhe primeiro arquivo


# Caminho da pasta
# folder_path = "C:\\Users\\Dell\\Desktop\\Cockpit_Edicao\\Para_Edicao\\1"





def find_folder_with_file(base_path):
    i = 1
    while True:
        folder_path = os.path.join(base_path, str(i))
        if os.path.exists(folder_path) and os.path.isdir(folder_path):
            # Lista todos os arquivos e diretórios na pasta atual
            for root, dirs, files in os.walk(folder_path):
                if files:  # Se houver algum arquivo na pasta
                    return folder_path
        i += 1

# Caminho base da pasta
# base_path = "C:\\Users\\User\\Desktop\\squad"
base_path = "C:\\Users\\rcasetta01\\Desktop\\Nova pasta\\Foto\\AGUARDANDO"


# Chama a função para procurar o arquivo
folder_path = find_folder_with_file(base_path)
if folder_path:
    print(f"Pasta contendo arquivos: {folder_path}")
else:
    print("Nenhuma pasta com arquivos foi encontrada.")



if folder_path:
    # Substitui "Para_Edicao" por "Editadas" no caminho
    modified_folder_path = folder_path.replace("AGUARDANDO", "EDITADAS")
    print(f"Pasta original contendo arquivos: {folder_path}")
    print(f"Pasta com nome modificado: {modified_folder_path}")
else:
    print("Nenhuma pasta com arquivos foi encontrada.")


# if folder_path:
#     # Substitui "Para_Edicao" por "Controle" no caminho
#     modified_folder_path_controle = folder_path.replace("squad", "controle")
#     print(f"Pasta original contendo arquivos: {folder_path}")
#     print(f"Pasta com nome modificado: {modified_folder_path_controle}")
# else:
#     print("Nenhuma pasta com arquivos foi encontrada.")    












# Listar todos os arquivos na pasta
files = os.listdir(folder_path)

# Filtrar a lista para incluir apenas arquivos (ignorar diretórios)
files = [f for f in files if os.path.isfile(os.path.join(folder_path, f))]

# Pegar o primeiro arquivo
if files:
    first_file = files[0]
    first_file_path = os.path.join(folder_path, first_file)
    
    # Digitar o caminho completo do primeiro arquivo usando pyautogui
    pyautogui.write(first_file_path)
    time.sleep(5)
    pyautogui.press('enter') 
    print("Primeiro arquivo digitado:", first_file_path)
else:
    print("A pasta está vazia ou não contém arquivos.")



########################################################Pula tutorial
time.sleep(30)
# wait = WebDriverWait(driver, 30)
# skip_tutorial_button = wait.until(EC.element_to_be_clickable((By.XPATH, "//button[@type='button' and contains(@class, 'text-[13px] leading-4 font-bold')]//span[text()='Skip tutorial']")))
# skip_tutorial_button.click()


# time.sleep(10)
# skip_tutorial_button = wait.until(
#     EC.visibility_of_element_located((By.XPATH, "//button[@type='button']//span[text()='Skip tutorial']"))
# )
# skip_tutorial_button = wait.until(
#     EC.element_to_be_clickable((By.XPATH, "//button[@type='button']//span[text()='Skip tutorial']"))
# )
# skip_tutorial_button.click()



# time.sleep(3)
########################################################Seleciona Blur
print("Selecionando blur...")
#wait = WebDriverWait(driver, 30)
#image = wait.until(EC.element_to_be_clickable((By.XPATH, "//img[@src='/images/icons/backgroundBlur.svg' and @class='w-9 h-9']")))
#image.click()
image_locator = (By.XPATH, "//img[@src='/images/icons/backgroundBlur.svg' and @class='w-9 h-9']")
image = WebDriverWait(driver, 50).until(presence_and_clickable_and_visible(image_locator))
image.click()

time.sleep(5)
#wait = WebDriverWait(driver, 30)
#element = wait.until(EC.element_to_be_clickable((By.XPATH, "//div[@class='flex bg-gray-450 rounded-[6px] w-[66px] h-[66px] border-2 justify-center border-transparent']//img[@alt='none']")))
#element.click()
image_locator = (By.XPATH, "//div[@class='flex bg-gray-450 rounded-[6px] w-[66px] h-[66px] border-2 justify-center border-transparent']//img[@alt='none']")
image = WebDriverWait(driver, 30).until(presence_and_clickable_and_visible(image_locator))
image.click()

########################################################Seleciona autoColor
time.sleep(5)
print("Escolhendo autoColor...")
#wait = WebDriverWait(driver, 30)
#image = wait.until(EC.element_to_be_clickable((By.XPATH, "//img[@src='/images/icons/autoColor.svg' and @class='w-9 h-9']")))
#image.click()
image_locator = (By.XPATH, "//img[@src='/images/icons/autoColor.svg' and @class='w-9 h-9']")
image = WebDriverWait(driver, 30).until(presence_and_clickable_and_visible(image_locator))
image.click()

time.sleep(5)
#wait = WebDriverWait(driver, 30)
#image = wait.until(EC.element_to_be_clickable((By.XPATH, "//img[@src='/images/thumbnails/steady.webp' and @class='object-cover w-[66px] h-[66px] rounded-[6px] border-2 border-transparent ']")))
#image.click()
image_locator = (By.XPATH, "//img[@src='/images/thumbnails/steady.webp' and @class='object-cover w-[66px] h-[66px] rounded-[6px] border-2 border-transparent ']")
image = WebDriverWait(driver, 30).until(presence_and_clickable_and_visible(image_locator))
image.click()
time.sleep(8)



########################################################Dowload primeiro passo
time.sleep(5)
wait = WebDriverWait(driver, 30)
download_button = wait.until(EC.element_to_be_clickable((By.ID, "tutorial-bulk-step3")))
download_button.click()
time.sleep(10)


########################################################Aplica as mudanças
wait = WebDriverWait(driver, 30)
apply_button = wait.until(EC.element_to_be_clickable((By.XPATH, "//button[contains(@class, 'w-11/12 btn btn--large shadow-dark-button btn--black mt-7 inline-flex')]//span[contains(@class, 'SB16')]//span[text()='Apply']")))
apply_button.click()
time.sleep(5)
print("Alterações aplicadas!")
time.sleep(15)


########################################################Dowload segundo passo
wait = WebDriverWait(driver, 30)
download_button = wait.until(EC.element_to_be_clickable((By.ID, "tutorial-bulk-step3")))
download_button.click()
print("Salvando foto...")


########################################################Downlaod terceiro passo
time.sleep(5)
wait = WebDriverWait(driver, 30)
download_button = wait.until(EC.element_to_be_clickable((By.XPATH, "//button[contains(@class, 'w-full h-9 btn btn--large btn--primary rounded-full border-0 mt-6')]//span[text()='Download']")))
download_button.click()



########################################################Escolhe pasta para o primeiro download
time.sleep(7)

# primeira_pasta_salvamento = r'C:\Users\Dell\Desktop\Cockpit_Edicao\Editados\1'
# caminho_completo = f'{primeira_pasta_salvamento}\\{first_file}'
caminho_completo = f'{modified_folder_path}\\{first_file}'
pyautogui.write(caminho_completo)
time.sleep(3)
pyautogui.press('enter')
# time.sleep(4)



########################################################Fecha caixa de diálogo do download
time.sleep(5)
pyautogui.press('esc')
print("Foto salva com sucesso!")



#######################################################Joga para a pasta controle

#Deleta o arquivo para nao repetir o loop
try:
    os.remove(first_file_path)
except OSError as e:
    print(f"Erro ao deletar o arquivo {first_file_path}: {e.strerror}")

# Diretórios de origem e destino
# origem = r'C:\Users\Dell\Desktop\Cockpit_Edicao\Para_Edicao\3'
# destino = r'C:\Users\Dell\Desktop\Cockpit_Edicao\Controle\3'

# origem = folder_path
# destino = modified_folder_path_controle

# # Listar os arquivos na pasta de origem
# lista_arquivos = os.listdir(origem)

# # Verificar se há arquivos na lista
# if lista_arquivos:
#     # Ordenar a lista de arquivos (por padrão, será ordenada alfabeticamente)
#     lista_arquivos.sort()

#     # Pegar o primeiro arquivo da lista (o primeiro arquivo alfabeticamente)
#     primeiro_arquivo = lista_arquivos[0]

#     # Caminho completo do arquivo de origem e destino
#     origem_arquivo = os.path.join(origem, primeiro_arquivo)
#     destino_arquivo = os.path.join(destino, primeiro_arquivo)

#     # Mover o arquivo para a pasta de destino
#     shutil.move(origem_arquivo, destino_arquivo)
#     print(f'Arquivo {primeiro_arquivo} movido de {origem} para {destino}')
# else:
#     print(f'Não há arquivos na pasta {origem}.')


############################################################################################################Sequencia a partir da nova tela


time.sleep(8)
print("Iniciando nova sequência...")





# # Diretório principal e subpastas
main_folder = r'C:\Users\rcasetta01\Desktop\Nova pasta\Foto\AGUARDANDO'
# control_folder = r'C:\Users\Dell\Desktop\Cockpit_Edicao\Controle'
edited_folder = r'C:\Users\rcasetta01\Desktop\Nova pasta\Foto\EDITADAS'
subfolders = os.listdir(main_folder)



# Diretório principal e subpastas
# main_folder = r'C:\Users\User\Desktop\squad'
# control_folder = r'C:\Users\User\Desktop\controle'
# edited_folder = r'C:\Users\User\Desktop\editadas'
# subfolders = os.listdir(main_folder)



# Iterar sobre as subpastas dentro de squad
for subfolder in subfolders:
    subfolder_path = os.path.join(main_folder, subfolder)

    # Verificar se é uma pasta
    if os.path.isdir(subfolder_path):
        # Continuar enquanto houver arquivos na subpasta
        while os.listdir(subfolder_path):
            # Selecionar o primeiro arquivo na subpasta
            first_file_loop = os.listdir(subfolder_path)[0]
            file_path = os.path.join(subfolder_path, first_file_loop)

            ################### Realiza as edições
            # Botão de upload
            time.sleep(10)
            wait = WebDriverWait(driver, 30)
            image = wait.until(EC.element_to_be_clickable((By.XPATH, "//img[@src='/_next/static/media/upload--white.e68b14d4.svg' and @alt='']")))
            image.click()

            time.sleep(8)

            # Insere o novo arquivo
            pyautogui.write(file_path)
            pyautogui.press('enter')

            time.sleep(10)
            ######################################################## Seleciona Blur



            elemento_obstrutor_presente = True
            while elemento_obstrutor_presente:
                try:
                # Tente encontrar o elemento obstrutor
                    elemento_obstrutor = wait.until(EC.presence_of_element_located((By.CLASS_NAME, 'flex w-full h-full items-center justify-center section-foreground relative')))
                    print("Elemento obstrutor encontrado, aguardando...") 
                    time.sleep(300)
                except:
                # Se o elemento obstrutor não for encontrado, saia do loop
                    elemento_obstrutor_presente = False
                    print("Elemento obstrutor não encontrado, prosseguindo...")



            print("Selecionando blur...")
            time.sleep(5)
            #element = wait.until(EC.element_to_be_clickable((By.XPATH, "//div[@class='flex bg-gray-450 rounded-[6px] w-[66px] h-[66px] border-2 justify-center border-transparent']//img[@alt='none']")))
            #element.click()
            image_locator = (By.XPATH, "//div[@class='flex bg-gray-450 rounded-[6px] w-[66px] h-[66px] border-2 justify-center border-transparent']//img[@alt='none']")
            image = WebDriverWait(driver, 50).until(presence_and_clickable_and_visible(image_locator))
            image.click()

            ######################################################## Seleciona autoColor
            print("Escolhendo autoColor...")
            time.sleep(5)
            #image = wait.until(EC.element_to_be_clickable((By.XPATH, "//img[@src='/images/thumbnails/steady.webp' and @class='object-cover w-[66px] h-[66px] rounded-[6px] border-2 border-transparent ']")))
            #image.click()
            image_locator = (By.XPATH, "//img[@src='/images/thumbnails/steady.webp' and @class='object-cover w-[66px] h-[66px] rounded-[6px] border-2 border-transparent ']")
            image = WebDriverWait(driver, 50).until(presence_and_clickable_and_visible(image_locator))
            image.click()


            ######################################################## Download primeiro passo
            time.sleep(5)
            download_button = wait.until(EC.element_to_be_clickable((By.ID, "tutorial-bulk-step3")))
            download_button.click()
            time.sleep(3)

            ######################################################## Aplica as mudanças
            time.sleep(5)
            apply_button = wait.until(EC.element_to_be_clickable((By.XPATH, "//button[contains(@class, 'w-11/12 btn btn--large shadow-dark-button btn--black mt-7 inline-flex')]//span[contains(@class, 'SB16')]//span[text()='Apply']")))
            apply_button.click()
            time.sleep(3)
            print("Alterações aplicadas!")
            time.sleep(15)

            ######################################################## Download segundo passo
            download_button = wait.until(EC.element_to_be_clickable((By.ID, "tutorial-bulk-step3")))
            download_button.click()
            print("Salvando foto...")

            ######################################################## Download terceiro passo
            time.sleep(5)
            download_button = wait.until(EC.element_to_be_clickable((By.XPATH, "//button[contains(@class, 'w-full h-9 btn btn--large btn--primary rounded-full border-0 mt-6')]//span[text()='Download']")))
            download_button.click()

            ######################################################## Define path salvamento editadas
            time.sleep(5)
            edited_subfolder = os.path.join(edited_folder, subfolder)
            if not os.path.exists(edited_subfolder):
                os.makedirs(edited_subfolder)
            edited_path = os.path.join(edited_subfolder, first_file_loop)

            # Printa o path de destino do arquivo na pasta editadas
            print(f"Arquivo será salvo em editadas: {edited_path}")

            time.sleep(5)
            pyautogui.write(edited_path)
            time.sleep(4)
            pyautogui.press('enter')

            ######################################################## Fecha caixa de diálogo do download
            time.sleep(5)
            pyautogui.press('esc')
            print("Foto salva com sucesso!")

            ######################################################## Mover o arquivo para a pasta de controle correspondente
            #Deleta o arquivo da pasta
            try:
                os.remove(file_path)
            except OSError as e:
                print(f"Erro ao deletar o arquivo {first_file_loop}: {e.strerror}")


            # time.sleep(5)
            # control_subfolder = os.path.join(control_folder, subfolder)
            # if not os.path.exists(control_subfolder):
            #     os.makedirs(control_subfolder)
            # dest_path = os.path.join(control_subfolder, first_file_loop)
            # print(f"Arquivo será movido para controle: {dest_path}")
            # shutil.move(file_path, dest_path)
            time.sleep(2)





print('Edições finalizadas com sucesso! :)')

driver.quit()
