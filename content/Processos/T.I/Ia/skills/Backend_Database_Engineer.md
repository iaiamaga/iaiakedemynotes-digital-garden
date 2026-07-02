## Backend / Database Engineer

When handling schema, migrations, or Supabase table design:

1. Write complete SQL — always include `id UUID DEFAULT gen_random_uuid() PRIMARY KEY`, `created_at TIMESTAMPTZ DEFAULT now()`, and foreign keys.
2. Use `auth.uid()` for user-scoped references — never raw strings.
3. Always include RLS policies alongside table creation.
4. For `saved_roadmaps`, the schema should be:

```sql
CREATE TABLE saved_roadmaps (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE NOT NULL,
  roadmap_name TEXT NOT NULL,
  nodes JSONB NOT NULL DEFAULT '[]',
  created_at TIMESTAMPTZ DEFAULT now(),
  updated_at TIMESTAMPTZ DEFAULT now()
);

ALTER TABLE saved_roadmaps ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can view own saved roadmaps"
ON saved_roadmaps FOR SELECT
USING (auth.uid() = user_id);

CREATE POLICY "Users can insert own saved roadmaps"
ON saved_roadmaps FOR INSERT
WITH CHECK (auth.uid() = user_id);

CREATE POLICY "Users can update own saved roadmaps"
ON saved_roadmaps FOR UPDATE
USING (auth.uid() = user_id);

CREATE POLICY "Users can delete own saved roadmaps"
ON saved_roadmaps FOR DELETE
USING (auth.uid() = user_id);
```
