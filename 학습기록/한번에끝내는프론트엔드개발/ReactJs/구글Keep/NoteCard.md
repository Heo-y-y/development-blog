# NoteCard 컴포넌트와 Note 데이터 생성

## fake note 데이터 생성

![스크린샷 2024-02-04 오후 6 05 59](https://github.com/Heo-y-y/development-blog/assets/112863029/58ec7b64-4124-4857-b69e-1100d344cc94)

![스크린샷 2024-02-04 오후 5 32 09](https://github.com/Heo-y-y/development-blog/assets/112863029/758083bb-6ddc-48c2-82ee-a0ce45c77ab4)

### noteData.ts

```tsx
import { v4 } from "uuid";

const notes = [
  {
    title: "Note 1 title",
    content: "Note 1 content",
    tags: [{ tag: "coding", id: v4() }],
    color: "#cce0ff",
    priority: "high",
    isPinned: true,
    isRead: false,
    data: "10/12/22 2.55 PM",
    createdTime: new Date("Sat Dec 10 2023 14:55:22").getTime(),
    editedTime: null,
    id: v4()
  },
  {
    title: "Note 2 title",
    content: "Note 2 content",
    tags: [{ tag: "exercise", id: v4() }],
    color: "#ffcccc",
    priority: "high",
    isPinned: true,
    isRead: false,
    data: "10/12/22 2.55 PM",
    createdTime: new Date("Sat Dec 10 2023 14:55:22").getTime(),
    editedTime: null,
    id: v4()
  }
]

export default notes;
```

### getAllNotes

```tsx
import { NoteCard } from "../components"
import { NotesContainer } from "../styles/styles"
import { Note } from "../types/note"

const getAllNotes = (mainNotes: Note[], filter: string) => {
  return (
    <>
    <div className="allNotes__notes-type">
      All Notes

    </div>
    <NotesContainer>
      {mainNotes.map((note) => (
        <NoteCard key={note.id} note={note} type="notes" />
      ))}
    </NotesContainer>
    </>
  )
}

export default getAllNotes;
```

### AllNotes

```tsx
<Box>
  {/* Notes */}
  {getAllNotes(mainNotes, filter)}
</Box>
```

## Note Card 컴포넌트 생성

```tsx
import React from 'react'
import { Card, ContentBox, FooterBox, TagsBox, TopBox } from './NoteCard.Styles';
import { NotesIconBox } from '../../styles/styles';
import { BsFillPinFill } from 'react-icons/bs';
import { Note } from '../../types/note';

interface NoteCardProps {
  note: Note,
  type: string
}

const NoteCard = ({note, type}: NoteCardProps) => {

  const { title, content, tags, color, priority, date, isPinned, isRead, id } = note;
  
  return (
    <Card style={{background: color}}>
      <TopBox>
        <div
          className='noteCard__title'
        >
          {title.length > 10 ? title.slice(0, 10) + '...' : title}
        </div>
        <div className='noteCard__top-options'>
          <span className='noteCard__priority'>
            {priority}
          </span>

          {type !== "archive" && type !== "trash" &&(
            <NotesIconBox
              className='noteCard__pin'
            >
              <BsFillPinFill 
                style={{ color : isPinned ? "red" : ""}}
              />
            </NotesIconBox>
          )}
        </div>
      </TopBox>
      <ContentBox>
        {content}
      </ContentBox>
      <TagsBox>
        {tags.map(({tag, id}) => (
          <span key={id}>{tag}</span>
        ))}
      </TagsBox>

      <FooterBox>
        <div className='noteCard__date'>{date}</div>
      </FooterBox>
    </Card>
  )
}

export default NoteCard;
```

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
