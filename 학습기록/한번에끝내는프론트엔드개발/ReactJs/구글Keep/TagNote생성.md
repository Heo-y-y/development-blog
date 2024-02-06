# agNote 페이지 생성

### TagNotes.tsx

여기서는 먼저 `mainNotes`의 데이터를 가져온 다음에 `forEach`를 사용헤 name이랑 `tag`가 같은걸 `notes`에 넣어서 `MainWrapper`에 넣어준다.

```tsx
import React from 'react'
import { useAppSelector } from '../../hooks/redux'
import { useParams } from 'react-router-dom';
import { Container, EmptyMsgBox } from '../../styles/styles';
import { MainWrapper } from '../../components';

const TagNotes = () => {

  const {name} = useParams() as {name: string}

  const {mainNotes} = useAppSelector((state) => state.notesList);

  let notes: Note[] = [];
  mainNotes.forEach((note) => {
    if(note.tags.find(({tag}) => tag === name)) {
      notes.push(note);
    }
  })

  return (
    <Container>
      {notes.length === 0 ? (
        <EmptyMsgBox>노트가 없습니다.</EmptyMsgBox>
      ):
      (<MainWrapper notes={notes} type={name} />)}
    </Container>
  )
}

export default TagNotes
```

![스크린샷 2024-02-06 오후 3.51.25.png](https://github.com/Heo-y-y/development-blog/assets/112863029/d5098aa8-0eb1-4758-b113-3368766a5d90)

Note2에 exercise 태그가 있다. Exercise 태그로 넘어가면 마찬가지로 해당 노트가 존재하는 것을 학인할 수 있다.

![스크린샷 2024-02-06 오후 3.51.32.png](https://github.com/Heo-y-y/development-blog/assets/112863029/341d9a6d-4695-446d-9d2c-c6a3ff505d5f)

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
