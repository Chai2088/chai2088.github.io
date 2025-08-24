---
title: "Memory Manager"
category: [Optimization, Memory]
layout: page
image: /assets/img/CrashNBurn.png
order: 9
---
A low-level programming project explored the mechanics of Object Allocators and memory management. The project focused on how memory is allocated and deallocated at a fundamental level, working directly with bytes and pointers rather than high-level abstractions. It provided a hands-on understanding of the core principles behind how software manages system memory.

I also helped integrate this into one of the game projects I worked on <a href="/articles/virtualmayhem"> Virtual Mayhem</a>. We built this for objects that were often created and destroyed during gameplay, this helped the memory efficiency and boosted its performance, reducing runtime by up to 10%.

## Creating Pages
```c++
try {
    //When it reaches the limit it throws an exception
    if (mStats.mPagesInUse == mConfig.mMaxPages && mConfig.mMaxPages != 0)
    {
        throw OaException(OaException::E_NO_MEMORY, "");
    }
    PageList = new char[mStats.mPageSize];
    //Setting all the page to the unallocated pattern
    std::memset(PageList, UNALLOCATED_PATTERN, mStats.mPageSize);
    //Connect the new page to the previous one
    GenericObject* page = reinterpret_cast<GenericObject*>(PageList);
    page->next = reinterpret_cast<GenericObject*>(prevPage);

    //Adds the left alignment
    char* left_align = PageList + sizeofPtr;
    std::memset(left_align, ALIGN_PATTERN, mConfig.mLeftAlignSize);
    CreateBlocks();

    mStats.mPagesInUse++;
    mStats.mFreeObjects += mConfig.mObjectsPerPage;
}
catch (std::bad_alloc&)
{
    throw OaException(OaException::E_NO_MEMORY, "");
}
```
Creates a new page and initializes the memory and then it creates the blocks inside the page
## Creating blocks
```c++
//Creating the blocks for the objects
GenericObject* prevObj = reinterpret_cast<GenericObject*>(PageList + start_offset);

//Create First header block with padding
char* mCurrentPointer = PageList + sizeofPtr + mConfig.mLeftAlignSize;
std::memset(mCurrentPointer, 0x00, mConfig.mHblockInfo.mSize);
mCurrentPointer += mConfig.mHblockInfo.mSize;
std::memset(mCurrentPointer, PAD_PATTERN, mConfig.mPadBytes);
prevObj->next = nullptr;
FreeList = reinterpret_cast<char*>(prevObj);

for (unsigned int i = 1; i < mConfig.mObjectsPerPage; ++i)
{
    FreeList += mStats.mObjectSize;

    //Add padding between the blocks
    std::memset(FreeList, PAD_PATTERN, block_offset - mStats.mObjectSize);
    FreeList += mConfig.mPadBytes;
    //Add alignment
    std::memset(FreeList, ALIGN_PATTERN, mConfig.mInterAlignSize);
    FreeList += mConfig.mInterAlignSize;
    //Create space for header blocks
    std::memset(FreeList, 0x00, mConfig.mHblockInfo.mSize);
    //Update FreeList pointer
    FreeList += mConfig.mPadBytes + mConfig.mHblockInfo.mSize;
    //Set the pointer to the next block
    GenericObject* currentObj = reinterpret_cast<GenericObject*>(FreeList);
    currentObj->next = prevObj;
    prevObj = currentObj;
}

//Adding the last padding
char* last_padding = FreeList + mStats.mObjectSize;
std::memset(last_padding, PAD_PATTERN, mConfig.mPadBytes);
```
Separates the page into smaller blocks and then it updates the freelist to add the newly created blocks. Every time an allocation is made it takes one block from the freelist and uses it for the object.
## Deallocation
```c++
//When we are deallocating and there is no free blocks
if (FreeList == nullptr)
{
    FreeList = static_cast<char*>(object);
    GenericObject* currentObj = reinterpret_cast<GenericObject*>(object);
    currentObj->next = nullptr;
}
else // Updates the freelist
{
    GenericObject* currentObj = reinterpret_cast<GenericObject*>(object);
    currentObj->next = reinterpret_cast<GenericObject*>(FreeList);
    FreeList = static_cast<char*>(object);
}
```
Updates the freelist when deallocating an object