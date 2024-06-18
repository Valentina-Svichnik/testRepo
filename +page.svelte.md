```<svelte:head>
    <title>Документация</title>
    <meta name="description" content="About this app" />
</svelte:head>

<script>
    // @ts-nocheck

    import { beforeUpdate, onMount } from 'svelte';
    import SvelteMarkdown from 'svelte-markdown';
    import Plus from "svelte-radix/Plus.svelte";
    import { Button } from "$lib/components/ui/button/index.js";
    import * as Dialog from "$lib/components/ui/dialog/index.js";
    import { Input } from "$lib/components/ui/input/index.js";
    import { Label } from "$lib/components/ui/label/index.js";
    import { Textarea } from "$lib/components/ui/textarea/index.js";
  
    let files = [];
    let markdownContent = '';
    let currentFile = '';
    let newFileName = '';
    let newFileContent = '';
    let selectedFile;
    let edditedFile;
    const token = '';
  
    async function fetchFiles() {
        const response = await fetch('https://api.github.com/repos/Valentina-Svichnik/testRepo/contents');
        const data = await response.json();
        files = data.filter(file => file.type === 'file' && file.name.endsWith('.md'));
    }
  
    async function fetchFileContent(filePath) {
        const response = await fetch(`https://raw.githubusercontent.com/Valentina-Svichnik/testRepo/main/${filePath}`);
        const markdown = await response.text();
        markdownContent = markdown;

        selectedFile = filePath;
        edditedFile = filePath;
    }

    function utf8ToBase64(str) {
        return btoa(unescape(encodeURIComponent(str)));
    }

    async function createNewFile() {
    const response = await fetch('https://api.github.com/repos/Valentina-Svichnik/testRepo/contents/' + newFileName, {
      method: 'PUT',
      headers: {
        'Authorization': `Bearer ${token}`,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        message: 'Create new file ' + newFileName,
        content: utf8ToBase64(newFileContent) // base64 encode the content
      })
    });

    if (response.ok) {
        await fetchFiles();
        Swal.fire({
			icon: 'success',
			title: 'Новый раздел успешно создан',
            text: 'Для отображения изменений, пожалуйста, перезагрузите страницу',
			confirmButtonText: 'Закрыть',
			confirmButtonColor: "#18181B",
		});
      
      newFileName = '';
      newFileContent = '';
    } else {
      const errorData = await response.json();
      Swal.fire({
			icon: 'error',
			title: 'Ошибка :(',
            text: `${errorData.message}`,
			confirmButtonText: 'Закрыть',
			confirmButtonColor: "#18181B",
		});
    }
  }

  async function editFile(filePath, newFileName, content) {
        const fileShaResponse = await fetch(`https://api.github.com/repos/Valentina-Svichnik/testRepo/contents/${filePath}`, {
            headers: {
                'Authorization': `Bearer ${token}`,
            }
        });
        const fileShaData = await fileShaResponse.json();
        const sha = fileShaData.sha;

        if (filePath !== newFileName) {
            // Create new file with the new name
            const response = await fetch(`https://api.github.com/repos/Valentina-Svichnik/testRepo/contents/${newFileName}`, {
                method: 'PUT',
                headers: {
                    'Authorization': `Bearer ${token}`,
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    message: 'Edit file and rename to ' + newFileName,
                    content: utf8ToBase64(content),
                    sha: sha
                })
            });

            if (response.ok) {
                // Delete the old file
                const deleteResponse = await fetch(`https://api.github.com/repos/Valentina-Svichnik/testRepo/contents/${filePath}`, {
                    method: 'DELETE',
                    headers: {
                        'Authorization': `Bearer ${token}`,
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        message: 'Delete old file ' + filePath,
                        sha: sha
                    })
                });

                if (deleteResponse.ok) {
                    await fetchFiles();
                    Swal.fire({
                        icon: 'success',
                        title: 'Файл успешно отредактирован',
                        text: 'Для отображения изменений, пожалуйста, перезагрузите страницу',
                        confirmButtonText: 'Закрыть',
                        confirmButtonColor: "#18181B",
                    });
                    await fetchFileContent(newFileName);
                } else {
                    const errorData = await deleteResponse.json();
                    Swal.fire({
                        icon: 'error',
                        title: 'Ошибка при удалении старого файла :(',
                        text: `${errorData.message}`,
                        confirmButtonText: 'Закрыть',
                        confirmButtonColor: "#18181B",
                    });
                }
            } else {
                const errorData = await response.json();
                Swal.fire({
                    icon: 'error',
                    title: 'Ошибка при создании нового файла :(',
                    text: `${errorData.message}`,
                    confirmButtonText: 'Закрыть',
                    confirmButtonColor: "#18181B",
                });
            }
        } else {
            // Just update the file content
            const response = await fetch(`https://api.github.com/repos/Valentina-Svichnik/testRepo/contents/${newFileName}`, {
                method: 'PUT',
                headers: {
                    'Authorization': `Bearer ${token}`,
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    message: 'Edit file ' + newFileName,
                    content: utf8ToBase64(content),
                    sha: sha
                })
            });

            if (response.ok) {
                await fetchFiles();
                Swal.fire({
                    icon: 'success',
                    title: 'Файл успешно отредактирован',
                    text: 'Для отображения изменений, пожалуйста, перезагрузите страницу',
                    confirmButtonText: 'Закрыть',
                    confirmButtonColor: "#18181B",
                });
                await fetchFileContent(newFileName);
            } else {
                const errorData = await response.json();
                Swal.fire({
                    icon: 'error',
                    title: 'Ошибка при редактировании файла :(',
                    text: `${errorData.message}`,
                    confirmButtonText: 'Закрыть',
                    confirmButtonColor: "#18181B",
                });
            }
        }
    }
  
    onMount(async () => {
        await fetchFiles();
        if (files.length > 0) {
            currentFile = files[0].path;
            await fetchFileContent(currentFile);
        }
    });

</script>
  
<div class="flex *:pt-5">
    <div class="flex h-full w-96 border-r overflow-y-auto fixed px-2 *:mt-4">
        <ul class="w-full *:py-2 *:px-5 *:rounded *:ease-in *:duration-200">
            {#each files as file}
                <li on:click={() => fetchFileContent(file.path)} class="hover:bg-stone-100 dark:hover:bg-stone-800">
                    {file.name.slice(0, -3)}
                </li>
            {/each}
        </ul>
        <Dialog.Root>
            <Dialog.Trigger class="h-[20px]"><Button><Plus class="h-3 w-3" /></Button></Dialog.Trigger>
            <Dialog.Content class="sm:max-w-[1000px]">
              <Dialog.Header>
                <Dialog.Title>Создать новый раздел</Dialog.Title>
                <Dialog.Description>Новый раздел создает новый файл в репозитории</Dialog.Description>
              </Dialog.Header>
              <div class="grid gap-4 py-4">
                <div class="grid items-center gap-4">
                  <Label>Название файла, включая .md (например, "Название раздела.md")</Label>
                  <Input bind:value={newFileName}  class="col-span-3" />
                </div>
                <div class="grid  items-center gap-4">
                  <Label>Текст файла</Label>
                  <Textarea bind:value={newFileContent} placeholder="Добавьте текст файла здесь (формат md)" class="h-56"/>
                </div>
              </div>
              <Dialog.Footer>
                <Button type="submit" on:click={createNewFile}>Создать</Button>
              </Dialog.Footer>
            </Dialog.Content>
          </Dialog.Root>
        
    </div>
    <div class="ml-96 mt-4 px-5 w-full">
        <div class="markdown-container w-full" >
            <SvelteMarkdown source={markdownContent} />
        </div>

        <Dialog.Root>
            <Dialog.Trigger class="mt-5 mb-10"><Button>Редактировать</Button></Dialog.Trigger>
            <Dialog.Content class="sm:max-w-[1000px]">
              <Dialog.Header>
                <Dialog.Title>Редактировать раздел</Dialog.Title>
              </Dialog.Header>
              <div class="grid gap-4 py-4">
                <div class="grid items-center gap-4">
                  <Label>Название файла, включая .md (например, "Название раздела.md")</Label>
                  <Input bind:value={edditedFile}  class="col-span-3" />
                </div>
                <div class="grid  items-center gap-4">
                  <Label>Текст файла</Label>
                  <Textarea bind:value={markdownContent} placeholder="Добавьте текст файла здесь (формат md)" class="h-56"/>
                </div>
              </div>
              <Dialog.Footer>
                <Button type="submit" on:click={() => editFile(selectedFile, edditedFile, markdownContent)}>Сохранить</Button>
              </Dialog.Footer>
            </Dialog.Content>
          </Dialog.Root>
    </div>
</div>

  