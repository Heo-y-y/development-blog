# Archive 페이지 생성

![스크린샷 2024-02-06 오후 3.31.43.png](https://github.com/Heo-y-y/development-blog/assets/112863029/4f3fde44-8fd5-42e4-bf32-399842c410e5)

![스크린샷 2024-02-06 오후 3.31.29.png](https://github.com/Heo-y-y/development-blog/assets/112863029/b5c39509-03aa-4019-bf0b-34396dc0017e)

Archive는 따로 정렬 기능은 없다.

### ArciveNotes.tsx

`archiveNotes` 배열에 아이템이 하나도 없으면 노트가 없다고 표시하고, 아니면 `MainWrapper` 컴포넌트를 렌더링한다.

```tsx
import React from 'react'
import { useAppSelector } from '../../hooks/redux'
import { Container, EmptyMsgBox } from '../../styles/styles';
import { MainWrapper } from '../../components';

const ArchiveNotes = () => {

  const {archiveNotes} = useAppSelector((state) => state.notesList);

  return (
    <Container>
      {archiveNotes.length === 0 ?
      <EmptyMsgBox>노트가 없습니다.</EmptyMsgBox>
      :
      <MainWrapper notes={archiveNotes} type="archive" />
    }
    </Container>
  )
}

export default ArchiveNotes
```

### MainWrapper

`NoteCard`들을 `MainNotes` 배열에 있는 걸로 하나씩 순회하며 렌더링 해준다.

```tsx
import React from 'react'
import { Note } from '../../types/note'
import { NotesContainer } from '../../styles/styles';
import { NoteCard } from '..';

interface MainWrapperProps {
  notes: Note[];
  type: string;
}

const MainWrapper = ({notes, type}: MainWrapperProps) => {
  return (
    <NotesContainer>
      {notes.map((note) => (
        <NoteCard key={note.id} note={note} type={type} />
      ))}
    </NotesContainer>
  )
}

export default MainWrapper
```

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
