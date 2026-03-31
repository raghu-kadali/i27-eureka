“How do you maintain versioning across environments?”

💡 Answer:

“We promote the same Docker image (same commit tag) from dev → test → prod to ensure consistency.”



“Why not use build number instead of Git commit?”

💡 Best answer:

“Build number is Jenkins-specific, but Git commit is universal and directly linked to source code, making it better for traceability.” some times used build+branch etc 
