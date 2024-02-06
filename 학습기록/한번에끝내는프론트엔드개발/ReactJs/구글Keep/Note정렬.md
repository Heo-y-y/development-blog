# Note 정렬 모달 생성

![스크린샷 2024-02-05 오후 5 56 45](https://github.com/Heo-y-y/development-blog/assets/112863029/0b8c0370-dcc3-4e78-943e-ab4c774532f8)

### AllNotes.tsx

`viewFiltersModal`이 무엇인지 가져와서 `true`일 때 `filtersModal` 컴포넌트를 보여준다.

```tsx
const { viewFiltersModal } = useAppSelector((state) => state.modal);
```

```tsx
const filterHandler = (e: React.ChangeEvent<HTMLInputElement>) => {
    setFilter(e.target.value);
  }

  const clearHandler = () => {
    setFilter("");
  }
```

```tsx
<Container>
      {viewFiltersModal && (
        <FiltersModal
          handlerFilter={filterHandler}
          handleClear={clearHandler}
          filter={filter}
        />
      )}
```

### FiltersModal.tsx

```tsx
import React from 'react'
import { useAppDispatch } from '../../../hooks/redux';
import { DeleteBox, FixedContainer } from '../Modal.Styles';
import { Box, Container, TopBox } from './FiltersModal.styles';
import { toggleFiltersModal } from '../../../store/modal/modalSlice';
import { FaTimes } from 'react-icons/fa';

interface FiltersModalProps {
  handleFilter: (e: React.ChangeEvent<HTMLInputElement>) => void;
  handleClear: () => void;
  filter: string;
}

const FiltersModal = ({handleFilter, handleClear, filter }: FiltersModalProps) => {

  const dispatch = useAppDispatch();

  return (
    <FixedContainer>
      <Container>
        <DeleteBox
          onClick={() => dispatch(toggleFiltersModal(false))}
          className='filters__close'
        >
          <FaTimes />
        </DeleteBox>
        <TopBox>
          <div className='filters__title'>정렬</div>
          <small onClick={handleClear} className='filters__delete'>
            CLEAR
          </small>
        </TopBox>

        <Box>
          <div className='filters__subtitle'>PRIORITY</div>
          <div className='filters__check'>
            <input
              type="radio"
              name="filter"
              value="low"
              id="low"
              checked={filter === "low"}
              onChange={(e) => handleFilter(e)}
            />
            <label htmlFor='low'>Low to High</label>
          </div>
          <div className='filters__check'>
            <input
              type="radio"
              name="filter"
              value="high"
              id="high"
              checked={filter === "high"}
              onChange={(e) => handleFilter(e)}
            />
            <label htmlFor='low'>High to Low</label>
          </div>
        </Box>

        <Box>
          <div className='filters__subtitle'>DATE</div>
          <div className='filters__check'>
            <input
              type="radio"
              name="filter"
              value="latest"
              id="new"
              checked={filter === "latest"}
              onChange={(e) => handleFilter(e)}
            />
            <label htmlFor='new'>Sort by Latest</label>
          </div>
          <div className='filters__check'>
            <input
              type="radio"
              name="filter"
              value="created"
              id="create"
              checked={filter === "created"}
              onChange={(e) => handleFilter(e)}
            />
            <label htmlFor='create'>Sort by Created</label>
          </div>
          <div className='filters__check'>
            <input
              type="radio"
              name="filter"
              value="edited"
              id="edit"
              checked={filter === "edited"}
              onChange={(e) => handleFilter(e)}
            />
            <label htmlFor='edit'>Sort by Edited</label>
          </div>
        </Box>
      </Container>
    </FixedContainer>
  )
}

export default FiltersModal
```

## Note 정렬 기능 적용

### getAllNotes.tsx

pinned의 처리를 해주는 기능이다.

```tsx
import { NoteCard } from "../components"
import { NotesContainer } from "../styles/styles"
import { Note } from "../types/note"

const getAllNotes = (mainNotes: Note[], filter: string) => {

  const pinned = mainNotes.filter(({isPinned}) => isPinned);
  const normal = mainNotes.filter(({isPinned}) => !isPinned);

  // normal일 경우
  if(normal.length !== 0 && pinned.length === 0) {
    return (
      <>
      <div className="allNotes__notes-type">
        All Notes <span>({normal.length})</span>
      </div>
      <NotesContainer>
        {normal.map((note) => (
          <NoteCard key={note.id} note={note} type="notes" />
        ))}
      </NotesContainer>
      </>
    )
  }
  // pinned일 경우
  if(pinned.length !== 0 && normal.length === 0) {
    return (
      <>
      <div className="allNotes__notes-type">
        Pinned Notes <span>({pinned.length})</span>
      </div>
      <NotesContainer>
        {pinned.map((note) => (
          <NoteCard key={note.id} note={note} type="notes" />
        ))}
      </NotesContainer>
      </>
    )
  }

  // pinned와 normal 둘 다 있는 경우
  if(pinned.length !== 0 && normal.length !== 0) {
    return (
      <>
        <div>
          <div className="allNotes__notes-type">
            Pinned Notes <span>({pinned.length})</span>
          </div>
          <NotesContainer>
            {pinned.map((note) => (
              <NoteCard key={note.id} note={note} type="notes" />
            ))}
          </NotesContainer>
        </div>
        <div>
          <div className="allNotes__notes-type">
            All Notes <span>({normal.length})</span>
          </div>
          <NotesContainer>
            {normal.map((note) => (
              <NoteCard key={note.id} note={note} type="notes" />
            ))}
          </NotesContainer>
        </div>
      </>
    )
  }
}

export default getAllNotes;
```

### filteredNotes

Note들을 정렬 시켜주는 기능이다.

```tsx
const filteredNotes = (notes: Note[], filter: string) => {
  const lowPriority = notes.filter(({priority}) => priority === "low");
  const highPriority = notes.filter(({priority}) => priority === "high");

  if(filter === "low") {
    return [...lowPriority, ...highPriority];

  } else if(filter === "high") {
    return [...highPriority, ...lowPriority];

  } else if(filter === 'latest') {
    return notes.sort((a, b) => b.createdTime - a.createdTime);

  } else if(filter === "created") {
    return notes.sort((a, b) => a.createdTime - b.createdTime);

  } else if(filter === "edited") {
    const editedNotes = notes.filter(({ editedTime }) => editedTime);
    const normalNotes = notes.filter(({ editedTime }) => !editedTime);

    const sortEdited = editedNotes.sort((a, b) => ((b.editedTime as number) - (a.editedTime as number)));
    return [...sortEdited, ...normalNotes];

  } else {
    return notes;
  }
}
```

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
