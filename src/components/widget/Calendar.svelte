<script lang="ts">
import Icon from "@iconify/svelte";

type CalendarPost = {
	slug: string;
	title: string;
	date: string;
};

interface Props {
	posts?: CalendarPost[];
}

let { posts = [] }: Props = $props();

const monthNames = [
	"1月",
	"2月",
	"3月",
	"4月",
	"5月",
	"6月",
	"7月",
	"8月",
	"9月",
	"10月",
	"11月",
	"12月",
];
const weekDays = ["一", "二", "三", "四", "五", "六", "日"];
const now = new Date();
const todayYear = now.getFullYear();
const todayMonth = now.getMonth();
const todayDate = now.getDate();

let currentYear = $state(todayYear);
let currentMonth = $state(todayMonth);
let selectedDateKey: string | null = $state(null);

function formatDateKey(year: number, month: number, day: number) {
	return `${year}-${String(month + 1).padStart(2, "0")}-${String(day).padStart(2, "0")}`;
}

function getPostsForDate(dateKey: string) {
	return posts.filter((post) => post.date === dateKey);
}

function previousMonth() {
	currentMonth -= 1;
	if (currentMonth < 0) {
		currentMonth = 11;
		currentYear -= 1;
	}
	selectedDateKey = null;
}

function nextMonth() {
	currentMonth += 1;
	if (currentMonth > 11) {
		currentMonth = 0;
		currentYear += 1;
	}
	selectedDateKey = null;
}

function backToToday() {
	currentYear = todayYear;
	currentMonth = todayMonth;
	selectedDateKey = null;
}

function selectDate(dateKey: string) {
	selectedDateKey = selectedDateKey === dateKey ? null : dateKey;
}

const emptyCellsCount = $derived(
	(new Date(currentYear, currentMonth, 1).getDay() + 6) % 7,
);
const daysInMonth = $derived(
	new Date(currentYear, currentMonth + 1, 0).getDate(),
);
const calendarDays = $derived(
	Array.from({ length: daysInMonth }, (_, index) => {
		const day = index + 1;
		const dateKey = formatDateKey(currentYear, currentMonth, day);
		const dayPosts = getPostsForDate(dateKey);
		return {
			day,
			dateKey,
			posts: dayPosts,
			isToday:
				currentYear === todayYear &&
				currentMonth === todayMonth &&
				day === todayDate,
		};
	}),
);
const displayedPosts = $derived(
	selectedDateKey
		? getPostsForDate(selectedDateKey)
		: posts.filter((post) =>
				post.date.startsWith(
					`${currentYear}-${String(currentMonth + 1).padStart(2, "0")}`,
				),
			),
);
const isCurrentMonth = $derived(
	currentYear === todayYear && currentMonth === todayMonth,
);
</script>

<div class="calendar-shell pb-1 pt-1">
	<div class="mb-2 flex items-center justify-between">
		<div class="relative ml-4 flex items-center font-bold before:absolute before:-left-4 before:h-4 before:w-1 before:rounded-md before:bg-[var(--primary)]">
			<span class="select-none text-lg font-bold text-neutral-900 transition dark:text-neutral-100" data-calendar-title>
				{currentYear}年 {monthNames[currentMonth]}
			</span>
		</div>

		<div class="flex shrink-0 items-center gap-1">
			{#if !isCurrentMonth}
				<button
					type="button"
					class="rounded-md p-1.5 text-[var(--primary)] transition-colors hover:bg-[var(--btn-plain-bg-hover)]"
					onclick={backToToday}
					aria-label="回到今天"
				>
					<Icon icon="material-symbols:restart-alt-rounded" class="text-xl" />
				</button>
			{/if}
			<button
				type="button"
				class="rounded-md p-1.5 text-neutral-600 transition-colors hover:bg-[var(--btn-plain-bg-hover)] hover:text-[var(--primary)] dark:text-neutral-400"
				onclick={previousMonth}
				aria-label="上个月"
			>
				<Icon icon="material-symbols:arrow-back-ios-new-rounded" class="text-lg" />
			</button>
			<button
				type="button"
				class="rounded-md p-1.5 text-neutral-600 transition-colors hover:bg-[var(--btn-plain-bg-hover)] hover:text-[var(--primary)] dark:text-neutral-400"
				onclick={nextMonth}
				aria-label="下个月"
				data-calendar-next
			>
				<Icon icon="material-symbols:arrow-forward-ios-rounded" class="text-lg" />
			</button>
		</div>
	</div>

	<div class="mb-2 grid grid-cols-7 gap-1">
		{#each weekDays as day}
			<div class="py-1 text-center text-xs font-medium text-neutral-500 transition dark:text-neutral-400">
				{day}
			</div>
		{/each}
	</div>

	<div class="grid grid-cols-7 gap-1">
		{#each { length: emptyCellsCount } as _}
			<div class="aspect-square" aria-hidden="true"></div>
		{/each}
		{#each calendarDays as cell (cell.dateKey)}
			<button
				type="button"
				class:calendar-selected={selectedDateKey === cell.dateKey}
				class:calendar-today={cell.isToday && selectedDateKey !== cell.dateKey}
				class="calendar-day relative flex aspect-square items-center justify-center rounded-md border border-transparent text-sm text-neutral-700 transition-all duration-200 hover:bg-[var(--btn-plain-bg-hover)] dark:text-neutral-300"
				onclick={() => selectDate(cell.dateKey)}
				aria-label={`${cell.dateKey}${cell.posts.length ? `，${cell.posts.length} 篇文章` : ""}`}
				data-calendar-date={cell.dateKey}
			>
				{cell.day}
				{#if cell.posts.length > 0 && selectedDateKey !== cell.dateKey}
					<span class="absolute bottom-1 h-1 w-1 rounded-full bg-[var(--primary)]"></span>
				{/if}
			</button>
		{/each}
	</div>

	{#if displayedPosts.length > 0}
		<div class="mt-4 border-t border-neutral-200 pt-2 transition dark:border-neutral-700" data-calendar-posts>
			<div class="calendar-posts flex max-h-36 flex-col gap-1 overflow-y-auto">
				{#each displayedPosts as post}
					<a
						href={`/posts/${post.slug}/`}
						class="group flex items-center justify-between rounded-lg px-2 py-2 text-sm font-bold text-neutral-700 transition-colors hover:bg-[var(--btn-plain-bg-hover)] hover:text-[var(--primary)] dark:text-neutral-300"
					>
						<span class="truncate">{post.title}</span>
						<span class="ml-2 shrink-0 text-xs font-normal text-neutral-400 group-hover:text-[var(--primary)]">
							{Number(post.date.slice(5, 7))}-{Number(post.date.slice(8, 10))}
						</span>
					</a>
				{/each}
			</div>
		</div>
	{/if}
</div>

<style>
	.calendar-shell {
		min-height: 17.5rem;
	}

	.calendar-selected {
		border-color: transparent;
		background: var(--primary);
		color: white;
		font-weight: 700;
		box-shadow: 0 4px 12px color-mix(in oklab, var(--primary) 28%, transparent);
	}

	.calendar-today {
		border-color: var(--primary);
		background: color-mix(in oklab, var(--primary) 10%, transparent);
		color: var(--primary);
		font-weight: 700;
	}

	.calendar-posts {
		scrollbar-width: thin;
		scrollbar-color: rgb(156 163 175 / 45%) transparent;
	}
</style>
