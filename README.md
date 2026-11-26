
create table public.teacher_profiles (
  user_id uuid primary key
    references auth.users(id) on delete cascade,
  full_name text not null,
  employee_code text unique not null,
  email text unique not null,
  approval_status text not null default 'pending'
    check (approval_status in (
      'pending', 'approved', 'rejected'
    )),
  created_at timestamptz default now()
);

create table public.teacher_registration_reviews (
  id bigint generated always as identity primary key,
  teacher_id uuid not null
    references auth.users(id),
  reviewed_by uuid not null
    references auth.users(id),
  decision text not null
    check (decision in ('approved', 'rejected')),
  review_note text,
  reviewed_at timestamptz default now()
);
