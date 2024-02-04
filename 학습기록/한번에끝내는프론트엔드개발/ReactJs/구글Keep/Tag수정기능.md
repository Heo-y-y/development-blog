# Tag 수정 기능

![스크린샷 2024-02-04 오후 8 32 10](https://github.com/Heo-y-y/development-blog/assets/112863029/46fe246d-a2ed-4894-a69b-1d03547f52eb)

## modal 생성

Add Tag를 클릭하면 나오는 modal

### tagsHandler

```tsx
// 태그를 더하거나 빼는걸 처리
const tagsHandler = (tag: string, type: string) => {
	const newTag = tag.toLocaleLowerCase();

  if(type === 'add') {
     setAddedTags((prev) => [...prev, {tag: newTag, id: v4()}])
   } else {
     setAddedTags(addedTags.filter(({tag}) => tag !== newTag))
   }
}
```

### TagsModal.tsx

```tsx
interface TagsModalProps {
  type: string;
  addedTags?: Tag[];
  handleTags?: (tag: string, type: string) => void
}

const TagsModal = ({ type, addedTags, handleTags }: TagsModalProps) => {
  const dispatch = useAppDispatch();
  const { tagsList } = useAppSelector((state) => state.tags);
  const [inputText, setInputText] = useState('');
```

```tsx
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
```

## CreateNoteModal

![스크린샷1 2024-02-04 오후 8 37 57](https://github.com/Heo-y-y/development-blog/assets/112863029/dba62019-ed10-4d59-bb73-6da688d25c75)

위 TagsModal에서 취소하는 것이 아닌 NoteModal에서 Tag를 지우는 코드

```tsx
<AddedTagsBox>
  {addedTags.map(({tag, id}) => (
    <div key={id}>
      <span className='createNote__tag'>{tag}</span>
      <span className='createNote__tag-remove'
        onClick={() => tagsHandler(tag, 'remove')}
      >
        <FaTimes />
       </span>
     </div>
   ))}
</AddedTagsBox>
```

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
