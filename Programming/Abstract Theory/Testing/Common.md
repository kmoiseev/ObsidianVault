
##### In software development, we need a proccess to "anticipate unanticipated changes"
There are two technical foundations needed to cope with the unanticipated changes
- There must be constant testing to catch regression errors. So that new features can be added without breaking existing ones. Frequent manual testing is impractical, so they must be automated
-  We need to keep the code as simple as possible, so it’s easier to understand and modify. Developers spend far more time reading code than writing it, so that’s what we should optimize for.


---

## Levels of testing
**Acceptance**: Does the whole system work?  
**Integration**: Does our code work against code we can't change?  
Integration tests make sure that any abstractions we build over third-party code work as we expect
**Unit**: Do our objects do the right thing, are they convenient to work with?

---