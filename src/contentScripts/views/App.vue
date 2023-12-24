<script setup lang="ts">
import 'uno.css'
interface Plugin {
  name: string
  type: 'download' | 'online'
  onClick: (movieName: string) => void
}

const movieName = getMovieName()
const { downloadButtonContainer, onlineButtonContainer } = createContainer() || {}

const plugins: Plugin[] = [
  {
    name: 'BT天堂',
    type: 'download',
    onClick: (movieName: string) => {
      openUrlByForm('https://www.bt-tt.com/e/search/', 'post', {
        show: 'title,newstext',
        keyboard: movieName,
        searchType: '影视搜索',
      })
    },
  },
  {
    name: '迅雷电影天堂',
    type: 'download',
    onClick: (movieName: string) => {
      openUrlByForm('https://www.xl720.com/', 'get', {
        s: movieName,
      })
    },
  },
  {
    name: 'BT世界网',
    type: 'download',
    onClick: (movieName: string) => {
      openUrlByForm('https://www.btsj6.com', 'get', {
        s: movieName,
      })
    },
  },
  {
    name: '音范丝4K',
    type: 'download',
    onClick: (movieName: string) => {
      openUrlByForm('https://www.yinfans.me', 'get', {
        s: movieName,
      })
    },
  },
  {
    name: '磁力引擎',
    type: 'download',
    onClick: (movieName: string) => {
      openUrlByForm('http://eclyq.one', 'post', {
        q: movieName,
      })
    },
  },
  {
    name: 'LIBVIO',
    type: 'online',
    onClick: (movieName: string) => {
      openUrlByForm('https://www.libvio.vip/search/-------------.html', 'get', {
        wd: movieName,
      })
    },
  },
]

main()

function main() {
  if (!downloadButtonContainer || !onlineButtonContainer || !movieName)
    return

  plugins.forEach((plugin) => {
    if (plugin.type === 'download')
      downloadButtonContainer.append(createElement(plugin))
    else if (plugin.type === 'online')
      onlineButtonContainer.append(createElement(plugin))
  })
}

function createContainer() {
  const h1 = document.querySelector('h1')
  if (!h1) {
    console.error('未找到标题')
    return
  }
  const containerStyle = {
    fontSize: '11px',
    margin: '5px 0',
  }
  const titleStyle = {
    display: 'inline-block',
    fontSize: '14px',
    width: '5em',
  }
  const buttonContainerStyle = {
    display: 'inline-flex',
    justifyContent: 'flex-start',
    alignItems: 'center',
    gap: '4px',
  }

  const [downloadButtonContainer, onlineButtonContainer] = ['马上下载', '在线观看'].map((item) => {
    const container = document.createElement('div')
    Object.assign(container.style, containerStyle)
    h1.append(container)

    const title = document.createElement('span')
    title.textContent = `${item}:`
    Object.assign(title.style, titleStyle)
    container.append(title)

    const buttonContainer = document.createElement('div')
    Object.assign(buttonContainer.style, buttonContainerStyle)
    container.append(buttonContainer)

    return buttonContainer
  })

  return { downloadButtonContainer, onlineButtonContainer }
}

function createElement({ name, onClick }: Plugin): HTMLElement {
  const btn = document.createElement('button')
  btn.textContent = name
  Object.assign(btn.style, {
    padding: '5px 10px',
    border: '1px solid #fff',
    borderRadius: '3px',
    backgroundColor: '#007722',
    color: '#fff',
    cursor: 'pointer',
  })
  btn.addEventListener('click', () => onClick(movieName!))
  return btn
}

function getMovieName() {
  const title = document.querySelector('.related-info > h2')
  return title?.firstElementChild?.textContent?.replace('的剧情简介', '')
}

function createInput(name: string, value: string) {
  const input = document.createElement('input')
  input.name = name
  input.value = value
  return input
}

function openUrlByForm(url: string, method: 'get' | 'post', params: Record<string, string>) {
  const form = document.createElement('form')
  form.hidden = true

  for (const [key, value] of Object.entries(params))
    form.append(createInput(key, value))

  form.setAttribute('action', url)
  form.setAttribute('method', method)
  form.setAttribute('target', '_blank')

  document.body.append(form)
  form.submit()
  setTimeout(() => form.remove())
}
</script>

<template>
  <div hidden />
</template>
