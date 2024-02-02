# Tag를 위한 Modal 생성

![스크린샷 2024-02-02 오후 3 45 23](https://github.com/Heo-y-y/development-blog/assets/112863029/48c1943b-bc03-4e0f-8b07-67e7a31c423d)

### TagsModal

```tsx
import React, { useState } from 'react'
import { FaMinus, FaPlus, FaTimes } from 'react-icons/fa';
import { v4 } from 'uuid';
import { useAppDispatch, useAppSelector } from '../../../hooks/redux'
import { toggleTagsModal } from '../../../store/modal/modalSlice';
import { removeTags } from '../../../store/notesList/notesListSlice';
import { addTags, deleteTags } from '../../../store/tags/tagsSlice';
import { Tag } from '../../../types/tag';
import getStandardName from '../../../utils/getStandardName';
import { DeleteBox, FixedContainer } from '../Modal.Styles';
import { Box, StyledInput, TagsBox } from './TagsModal.Styles';

interface TagsModalProps {
  type: string;
  addedTags?: Tag[];
  handleTags?: (tag: string, type: string) => void
}

const TagsModal = ({ type, addedTags, handleTags }: TagsModalProps) => {
  const dispatch = useAppDispatch();
  const { tagsList } = useAppSelector((state) => state.tags);
  const [inputText, setInputText] = useState('');

  const submitHandler = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();

    if (!inputText) {
      return;
    }

    dispatch(addTags({ tag: inputText.toLocaleLowerCase(), id: v4() }));
    setInputText('');
  }

  const deleteTagsHandler = (tag: string, id: string) => {
    dispatch(deleteTags(id));
    dispatch(removeTags({ tag }));
  }

  return (
    <FixedContainer>
      <Box>
        <div className='editTags__header'>
          <div className='editTags__title'>
            {type === "add" ? "ADD" : "Edit"} Tags
          </div>
          <DeleteBox
            className='editTags__close'
            onClick={() => dispatch(toggleTagsModal({ type, view: false }))}
          >
            <FaTimes />
          </DeleteBox>
        </div>

        <form onSubmit={submitHandler}>
          <StyledInput
            type="text"
            value={inputText}
            placeholder="new tag..."
            onChange={(e) => setInputText(e.target.value)}
          />
        </form>
        <TagsBox>
          {tagsList.map(({ tag, id }) => (
            <li key={id}>
              <div className='editTags__tag'>
                {getStandardName(tag)}
              </div>
              {type === "edit" ? (
                <DeleteBox onClick={() => deleteTagsHandler(tag, id)}>
                  <FaTimes />
                </DeleteBox>
              ) : (
                <DeleteBox>
                  {addedTags?.find(
                    (addedTag: Tag) => addedTag.tag === tag.toLowerCase()
                  ) ? (
                    <FaMinus onClick={() => handleTags!(tag, "remove")} />
                  ) :
                    (
                      <FaPlus onClick={() => handleTags!(tag, "add")} />
                    )
                  }
                </DeleteBox>
              )}
            </li>
          ))}
        </TagsBox>
      </Box>
    </FixedContainer>
  )
}

export default TagsModal
```

### submitHandler

```tsx
const submitHandler = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();

    if (!inputText) {
      return;
    }

    dispatch(addTags({ tag: inputText.toLocaleLowerCase(), id: v4() }));
    setInputText('');
  }
```

### deleteTagsHandler

이미 노트에 할당된 태그를 지우는 핸들러이다.

```tsx
const deleteTagsHandler = (tag: string, id: string) => {
    dispatch(deleteTags(id));
    dispatch(removeTags({ tag }));
  }
```

### tagsSlice.ts

태그를 생성, 삭제, 중복일 경우 표시하는 Slice이다.

![스크린샷 2024-02-02 오후 4.30.04.png](https://github.com/Heo-y-y/development-blog/assets/112863029/4b0f6451-4165-4c6b-bc31-68213b32dd16)

![스크린샷 2024-02-02 오후 4.30.17.png](https://github.com/Heo-y-y/development-blog/assets/112863029/60f99e3c-92a7-4c9d-8f64-ca7ae0a757e3)

![스크린샷 2024-02-02 오후 4.30.28.png](https://github.com/Heo-y-y/development-blog/assets/112863029/3862acb2-2e92-476d-8623-706974befaf9)

```tsx
import { createSlice } from "@reduxjs/toolkit";
import { toast } from "react-toastify";
import { v4 } from 'uuid';

const initialState = {
  tagsList: [
    { tag: "coding", id: v4() },
    { tag: "exercise", id: v4() },
    { tag: "quotes", id: v4() }
  ]
}

const tagsSlice = createSlice({
  name: "tags",
  initialState,
  reducers: {
    addTags: (state, { payload }) => {
      if (state.tagsList.find(({tag}) => tag === payload.tag)) {
        toast.warning("이미 존재하는 태그입니다.");
      } else {
        state.tagsList.push(payload);
        toast.info("새로운 태그가 등록되었습니다.");
      }
    },
    deleteTags: (state, { payload }) => {
      state.tagsList = state.tagsList.filter(({id}) => id !== payload)
      toast.info("태그가 삭제되었습니다.");
    },
  }
})

export const {addTags, deleteTags} = tagsSlice.actions;

export default tagsSlice.reducer;
```

### notesListSlice.ts

```tsx
import { createSlice } from "@reduxjs/toolkit";
import { Note } from "../../types/note";

interface NoteState {
  mainNotes: Note[],
  archiveNotes: Note[],
  trashNotes: Note[],
  editNote: null | Note[]
}

const initialState: NoteState = {
  mainNotes: [],
  archiveNotes: [],
  trashNotes: [],
  editNote: null
}

const notesListSlice = createSlice({
  name: "noteList",
  initialState,
  reducers: {
    removeTags: (state, {payload}) => {
      state.mainNotes = state.mainNotes.map((note) => ({
        ...note,
        tags: note.tags.filter(({tag}) => tag !== payload.tag)
      }))
    }
  }
})

export const { removeTags } = notesListSlice.actions;

export default notesListSlice.reducer;
```

### App.tsx

```tsx
import 'react-toastify/dist/ReactToastify.css'

function App() {

  const { viewEditTagsModal } = useAppSelector(state => state.modal);

  return (
      <div className='App'>

      {viewEditTagsModal && <TagsModal type='edit' />}

      <ToastContainer 
        position='bottom-right'
        theme='light'
        pauseOnHover
        autoClose={1500}
      />
```

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
