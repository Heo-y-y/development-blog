# Trash 페이지 생성

![스크린샷 2024-02-06 오후 3.39.53.png](https://github.com/Heo-y-y/development-blog/assets/112863029/3f582b52-a2f0-4182-9a1d-4fc2f5faf19b)

![스크린샷 2024-02-06 오후 3.40.04.png](https://github.com/Heo-y-y/development-blog/assets/112863029/e4a49d5d-c907-4653-b185-6a9622bcbe83)

### TrashNote.tsx

Trash도 Archive랑 거의 비슷한데, `TrashNote`의 배열만 가져오는 부분과 `note={}`에 `trashNotes`만 넣어주는 것만 다르다.

```tsx
import React from 'react'
import { useAppSelector } from '../../hooks/redux'
import { Container, EmptyMsgBox } from '../../styles/styles';
import { MainWrapper } from '../../components';

const TrashNotes = () => {

  const {trashNotes} = useAppSelector((state) => state.notesList);

  return (
    <Container>
     {trashNotes.length === 0 ? (
        <EmptyMsgBox>노트가 없습니다.</EmptyMsgBox>
     ):
      (
        <MainWrapper notes={trashNotes} type="trash" />
      )
     }
    </Container>
  )
}

export default TrashNotes
```

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
